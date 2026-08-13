<!DOCTYPE html>
<html lang="en" class="h-full bg-slate-950 text-slate-100">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Hydrogel ESR Lab Tracker & Calculator (LCST / UCST Thermo-Responsive Engine)</title>
  
  <script src="https://cdn.tailwindcss.com"></script>
  <script src="https://unpkg.com/lucide@latest"></script>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;500;600;700;800&family=JetBrains+Mono:wght@400;500;600&display=swap" rel="stylesheet">
  
  <script>
    tailwind.config = {
      darkMode: 'class',
      theme: {
        extend: {
          fontFamily: {
            sans: ['"Plus Jakarta Sans"', 'sans-serif'],
            mono: ['"JetBrains Mono"', 'monospace'],
          },
          colors: {
            teal: {
              400: '#2dd4bf',
              500: '#14b8a6',
              600: '#0d9488',
              900: '#134e4a',
            }
          }
        }
      }
    };
  </script>

  <style>
    ::-webkit-scrollbar {
      width: 6px;
      height: 6px;
    }
    ::-webkit-scrollbar-track {
      background: rgba(15, 23, 42, 0.6);
    }
    ::-webkit-scrollbar-thumb {
      background: rgba(45, 212, 191, 0.3);
      border-radius: 9999px;
    }
    ::-webkit-scrollbar-thumb:hover {
      background: rgba(45, 212, 191, 0.6);
    }

    /* Ambient background glow accents */
    .bg-radial-glow {
      background: 
        radial-gradient(circle at 10% 15%, rgba(20, 184, 166, 0.12) 0%, transparent 35%),
        radial-gradient(circle at 85% 25%, rgba(168, 85, 247, 0.12) 0%, transparent 40%),
        radial-gradient(circle at 50% 80%, rgba(236, 72, 153, 0.1) 0%, transparent 45%);
    }
  </style>
</head>
<body class="h-full font-sans antialiased bg-slate-950 text-slate-100 flex flex-col overflow-x-hidden bg-radial-glow relative">

  <!-- TOP HEADER BANNER -->
  <header class="border-b border-slate-800 bg-slate-900/90 backdrop-blur-md sticky top-0 z-30 px-4 lg:px-8 py-3.5 flex flex-wrap items-center justify-between gap-4">
    <div class="flex items-center space-x-3">
      <div class="w-10 h-10 rounded-xl bg-gradient-to-tr from-teal-400 via-cyan-400 to-fuchsia-500 flex items-center justify-center shadow-lg shadow-teal-500/25">
        <i data-lucide="flask-conical" class="w-5 h-5 text-slate-950 stroke-[2.5]"></i>
      </div>
      <div>
        <div class="flex items-center gap-2">
          <h1 class="font-extrabold text-lg tracking-tight bg-gradient-to-r from-teal-300 via-cyan-300 to-fuchsia-300 bg-clip-text text-transparent">HydroGel<span class="text-teal-400">ESR</span></h1>
          <span class="text-[10px] uppercase font-mono px-2 py-0.5 rounded-full bg-gradient-to-r from-teal-500/20 to-cyan-500/20 text-teal-300 border border-teal-500/40">v3.5 Auto-Thermo LCST/UCST Engine</span>
        </div>
        <p class="text-xs text-slate-400">Equilibrium Swelling, Thermo-Responsive Kinetics & Thermodynamics ($25\,^{\circ}\text{C} - 100\,^{\circ}\text{C}$)</p>
      </div>
    </div>

    <div class="flex items-center space-x-3 sm:space-x-4">
      <div class="text-right bg-emerald-950/40 border border-emerald-500/30 px-3 py-1 rounded-xl">
        <span class="text-[9px] uppercase tracking-wider text-emerald-400 block font-semibold">Protocol Progress</span>
        <span id="header-progress-text" class="text-xs font-mono font-bold text-emerald-300">0 / 0 Done</span>
      </div>
      <div class="text-right bg-violet-950/40 border border-violet-500/30 px-3 py-1 rounded-xl">
        <span class="text-[9px] uppercase tracking-wider text-violet-400 block font-semibold">Logged Groups</span>
        <span id="header-sample-count" class="text-xs font-mono font-bold text-violet-300">0 Groups</span>
      </div>
      <div class="text-right bg-amber-950/40 border border-amber-500/30 px-3 py-1 rounded-xl">
        <span class="text-[9px] uppercase tracking-wider text-amber-400 block font-semibold">Avg. ESR % (&plusmn;SD)</span>
        <span id="header-avg-esr" class="text-xs font-mono font-bold text-amber-300">0.00%</span>
      </div>

      <button onclick="window.openExportModal()" class="px-3.5 py-1.5 text-xs font-semibold rounded-xl bg-gradient-to-r from-purple-600 to-pink-600 hover:from-purple-500 hover:to-pink-500 text-white border border-pink-400/30 shadow-md shadow-pink-500/20 transition-all flex items-center gap-1.5">
        <i data-lucide="download" class="w-3.5 h-3.5 text-white"></i>
        Export Data
      </button>
    </div>
  </header>

  <main class="flex-1 p-4 lg:p-6 max-w-7xl w-full mx-auto grid grid-cols-1 lg:grid-cols-12 gap-6">

    <!-- LEFT PANEL: Experimental Protocol & Literature Reference Module (5 Cols) -->
    <section class="lg:col-span-5 flex flex-col space-y-4">
      
      <!-- Protocol Checklist Card -->
      <div class="bg-slate-900/90 border border-slate-800/80 rounded-2xl p-4 flex flex-col shadow-xl backdrop-blur-sm">
        
        <div class="flex items-center justify-between pb-3 border-b border-slate-800">
          <div class="flex items-center space-x-2">
            <i data-lucide="check-square" class="w-4 h-4 text-emerald-400"></i>
            <h2 class="text-sm font-bold text-slate-100 tracking-tight">Experiment Task Checklist</h2>
          </div>
          <button onclick="window.openAddTaskModal()" class="px-2.5 py-1 text-xs font-semibold rounded-lg bg-emerald-500/10 hover:bg-emerald-500/20 text-emerald-300 border border-emerald-500/30 transition-all flex items-center gap-1">
            <i data-lucide="plus" class="w-3.5 h-3.5"></i> Add Task
          </button>
        </div>

        <div class="py-3 space-y-1.5">
          <div class="flex justify-between items-center text-xs">
            <span class="text-slate-400 font-medium">Protocol Completion</span>
            <span id="task-progress-percent" class="font-mono text-emerald-400 font-bold">0%</span>
          </div>
          <div class="w-full bg-slate-950 h-2.5 rounded-full overflow-hidden border border-slate-800/80">
            <div id="task-progress-bar" class="bg-gradient-to-r from-emerald-400 via-teal-400 to-cyan-400 h-full w-0 transition-all duration-300 shadow-sm"></div>
          </div>
        </div>

        <div class="flex space-x-1 border-b border-slate-800 pb-2 text-xs font-medium text-slate-400 overflow-x-auto">
          <button onclick="window.filterTasks('all')" id="task-tab-all" class="px-3 py-1 rounded-lg bg-teal-500/20 text-teal-300 border border-teal-500/30 font-semibold whitespace-nowrap">All Tasks</button>
          <button onclick="window.filterTasks('prep')" id="task-tab-prep" class="px-3 py-1 rounded-lg hover:text-purple-300 whitespace-nowrap border border-transparent">1. Synthesis/Prep</button>
          <button onclick="window.filterTasks('weighing')" id="task-tab-weighing" class="px-3 py-1 rounded-lg hover:text-amber-300 whitespace-nowrap border border-transparent">2. Weighing & Swelling</button>
          <button onclick="window.filterTasks('kinetics')" id="task-tab-kinetics" class="px-3 py-1 rounded-lg hover:text-cyan-300 whitespace-nowrap border border-transparent">3. Analysis & Math</button>
        </div>

        <div id="tasks-list" class="mt-3 space-y-2.5 max-h-[380px] overflow-y-auto pr-1">
          <!-- Populated via JS -->
        </div>

        <div class="mt-4 pt-3 border-t border-slate-800 flex justify-between items-center text-xs">
          <button onclick="window.loadStandardProtocol()" class="text-slate-400 hover:text-teal-400 flex items-center gap-1 transition-colors">
            <i data-lucide="rotate-ccw" class="w-3.5 h-3.5"></i> Reset Standard Protocol
          </button>
          <button onclick="window.clearCompletedTasks()" class="text-slate-500 hover:text-rose-400 transition-colors">
            Clear Checked
          </button>
        </div>

      </div>

      <!-- Literature References & Thermodynamic Framework Card -->
      <div class="bg-slate-900/90 border border-slate-800 rounded-2xl p-4 text-xs space-y-3 shadow-xl">
        <div class="flex items-center justify-between border-b border-slate-800 pb-2">
          <div class="flex items-center gap-2 text-slate-100 font-bold">
            <i data-lucide="book-open" class="w-4 h-4 text-cyan-400"></i>
            <span>Thermo-Responsive Hydrogel Literature & Theory</span>
          </div>
          <span class="text-[10px] font-mono text-cyan-300 bg-cyan-500/10 border border-cyan-500/30 px-2 py-0.5 rounded-full">Flory-Rehner & Boltzmann</span>
        </div>

        <!-- Thermodynamics Summary -->
        <div class="space-y-2 text-[11px] text-slate-300">
          <div class="p-2.5 bg-slate-950/80 rounded-xl border border-slate-800/80 space-y-1">
            <div class="font-bold text-teal-300 flex items-center justify-between">
              <span>1. LCST Mechanism (Entropy Driven)</span>
              <span class="text-[9px] font-mono bg-teal-500/20 px-1.5 py-0.5 rounded text-teal-200">$\Delta G_{mix} = \Delta H_{mix} - T\Delta S_{mix}$</span>
            </div>
            <p class="text-slate-400 text-[10.5px]">
              Below LCST ($T < T_c$), hydrogen bonding between water and hydrophilic groups (e.g., amide C=O, N-H) dominates ($\Delta H_{mix} < 0$). Above LCST, ordered iceberg water cages melt ($\Delta S_{mix} < 0$), hydrophobic interactions dominate, and hydrogel collapses into a globule.
            </p>
          </div>

          <div class="p-2.5 bg-slate-950/80 rounded-xl border border-slate-800/80 space-y-1">
            <div class="font-bold text-amber-300 flex items-center justify-between">
              <span>2. UCST Mechanism (Enthalpy Driven)</span>
              <span class="text-[9px] font-mono bg-amber-500/20 px-1.5 py-0.5 rounded text-amber-200">$T > T_c \implies \text{Swelled}$</span>
            </div>
            <p class="text-slate-400 text-[10.5px]">
              Below UCST ($T < T_c$), strong inter-polymer hydrogen bonds or zwitterionic/electrostatic physical crosslinks keep the network collapsed. Heating provides thermal energy to break inter-chain complexes ($\Delta H_{break} > 0$), resulting in sudden swelling.
            </p>
          </div>

          <!-- Flory Rehner Thermodynamics -->
          <div class="p-2.5 bg-slate-950/80 rounded-xl border border-slate-800/80 space-y-1 font-mono text-[10px]">
            <span class="text-pink-300 font-bold block">Flory-Rehner Swelling Equation:</span>
            <div class="bg-slate-900 p-2 rounded border border-slate-800 text-center text-pink-200 font-semibold my-1 overflow-x-auto">
              $\ln(1 - v_2) + v_2 + \chi(T) v_2^2 + \frac{V_1}{\bar{v} M_c} \left(v_2^{1/3} - \frac{v_2}{2}\right) = 0$
            </div>
            <span class="text-slate-400 text-[9.5px] block">
              Where $v_2 = Q_v^{-1}$ is the polymer volume fraction, $V_1$ is solvent molar volume, $M_c$ is molecular weight between crosslinks, and $\chi(T)$ is the Flory-Huggins parameter: $\chi(T) \approx \frac{1}{2} - A\left(1 - \frac{\Theta}{T}\right)$.
            </span>
          </div>
        </div>

        <!-- Curated Literature References -->
        <div class="pt-2 border-t border-slate-800 space-y-2">
          <span class="text-[10px] uppercase font-mono text-slate-400 font-semibold block">Key Primary Literature References:</span>
          
          <ul class="space-y-1.5 text-[10.5px] text-slate-300 font-sans">
            <li class="p-2 rounded-lg bg-slate-950/50 border border-slate-800/60 hover:border-teal-500/30 transition-colors">
              <span class="font-bold text-teal-300 block">Heskins, M., & Guillet, J. E. (1968)</span>
              <span class="text-slate-400 block">Solution Properties of Poly(N-isopropylacrylamide). <em>J. Macromol. Sci. Chem.</em>, 2(8), 1441–1455.</span>
              <span class="text-[9px] font-mono text-slate-500">First foundational LCST phase transition report for PNIPAM (~32 °C).</span>
            </li>

            <li class="p-2 rounded-lg bg-slate-950/50 border border-slate-800/60 hover:border-amber-500/30 transition-colors">
              <span class="font-bold text-amber-300 block">Seuring, J., & Agarwal, S. (2012)</span>
              <span class="text-slate-400 block">Polymers with UCST in Aqueous Solution. <em>Macromol. Rapid Commun.</em>, 33(22), 1898–1920.</span>
              <span class="text-[9px] font-mono text-slate-500">Comprehensive review of UCST H-bonding and zwitterionic network mechanisms.</span>
            </li>

            <li class="p-2 rounded-lg bg-slate-950/50 border border-slate-800/60 hover:border-pink-500/30 transition-colors">
              <span class="font-bold text-pink-300 block">Schild, H. G. (1992)</span>
              <span class="text-slate-400 block">Poly(N-isopropylacrylamide): Experiment, theory and application. <em>Prog. Polym. Sci.</em>, 17(2), 163–249.</span>
              <span class="text-[9px] font-mono text-slate-500">Definitive equilibrium swelling ratio and structural transition mechanisms.</span>
            </li>

            <li class="p-2 rounded-lg bg-slate-950/50 border border-slate-800/60 hover:border-cyan-500/30 transition-colors">
              <span class="font-bold text-cyan-300 block">Peppas, N. A., et al. (2000)</span>
              <span class="text-slate-400 block">Hydrogels in pharmaceutical applications. <em>Eur. J. Pharm. Biopharm.</em>, 50(1), 27–46.</span>
              <span class="text-[9px] font-mono text-slate-500">Mathematical modeling of swelling dynamics, crosslink density, and equilibrium mass ratios.</span>
            </li>
          </ul>
        </div>

      </div>

    </section>

    <!-- RIGHT PANEL: Multi-replicate Calculator, Tables, Kinetics & Dynamic Thermo Canvas (7 Cols) -->
    <section class="lg:col-span-7 flex flex-col space-y-6">

      <!-- Multi-Replicate Calculator Form -->
      <div class="bg-slate-900/90 border border-slate-800 rounded-2xl p-5 shadow-xl space-y-4">
        
        <div class="flex items-center justify-between border-b border-slate-800 pb-3">
          <div class="flex items-center space-x-2">
            <i data-lucide="calculator" class="w-4 h-4 text-cyan-400"></i>
            <h2 class="text-sm font-bold text-slate-100">Multi-Replicate Hydrogel Calculator</h2>
          </div>
          <div class="flex items-center gap-2">
            <label class="text-[11px] text-slate-400 font-mono">Replicates (N):</label>
            <select id="input-replicate-count" onchange="window.renderReplicateInputs()" class="bg-slate-950 border border-slate-800 rounded-lg px-2 py-1 text-xs text-teal-300 font-mono focus:outline-none">
              <option value="1">1 (Single)</option>
              <option value="3" selected>3 (Triplicate)</option>
              <option value="4">4 Replicates</option>
              <option value="5">5 Replicates</option>
            </select>
          </div>
        </div>

        <form id="sample-form" onsubmit="window.handleSampleSubmit(event)" class="space-y-3 text-xs">
          <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-3">
            <div>
              <label class="block text-slate-400 font-medium mb-1">Group / Sample ID *</label>
              <input type="text" id="input-sample-id" required placeholder="e.g. GEL-PNIPAM-LCST" class="w-full bg-slate-950 border border-slate-800 focus:border-teal-400 rounded-xl px-3 py-2 text-slate-100 focus:outline-none">
            </div>

            <div>
              <label class="block text-slate-400 font-medium mb-1">Polymer Composition</label>
              <input type="text" id="input-composition" placeholder="e.g. PNIPAM / BIS 100:1" class="w-full bg-slate-950 border border-slate-800 focus:border-teal-400 rounded-xl px-3 py-2 text-slate-100 focus:outline-none">
            </div>

            <div>
              <label class="block text-slate-400 font-medium mb-1">Thermo Profile *</label>
              <select id="input-responsive-type" onchange="window.handleResponsiveTypeChange()" class="w-full bg-slate-950 border border-slate-800 focus:border-teal-400 rounded-xl px-2.5 py-2 text-teal-300 font-mono font-semibold focus:outline-none">
                <option value="lcst" selected>LCST (PNIPAM type)</option>
                <option value="ucst">UCST (PAAm/AAc IPN)</option>
                <option value="non-responsive">Non-Responsive (PEGDA)</option>
              </select>
            </div>

            <div>
              <label class="block text-slate-400 font-medium mb-1">Transition $T_c$ (°C)</label>
              <input type="number" step="0.1" id="input-transition-temp" value="32.5" placeholder="e.g. 32.5" class="w-full bg-slate-950 border border-slate-800 focus:border-teal-400 rounded-xl px-3 py-2 text-amber-300 font-mono focus:outline-none">
            </div>
          </div>

          <!-- Medium Input -->
          <div>
            <label class="block text-slate-400 font-medium mb-1">Swelling Medium / Buffer</label>
            <input type="text" id="input-medium" placeholder="PBS pH 7.4, Deionized Water" value="PBS pH 7.4" class="w-full bg-slate-950 border border-slate-800 focus:border-teal-400 rounded-xl px-3 py-2 text-slate-100 focus:outline-none">
          </div>

          <!-- Dynamic Replicate Rows Container -->
          <div class="bg-slate-950/60 border border-slate-800/80 rounded-xl p-3 space-y-2">
            <div class="flex items-center justify-between text-[11px] font-mono text-slate-400 px-1 border-b border-slate-800/80 pb-1.5">
              <span>Replicate #</span>
              <span>Dry Weight W<sub>d</sub> (mg)</span>
              <span>Swollen Weight W<sub>s</sub> (mg)</span>
              <span class="text-teal-400">ESR (%)</span>
              <span class="text-cyan-300">EWC (%)</span>
            </div>

            <div id="replicates-container" class="space-y-2">
              <!-- Rendered dynamically -->
            </div>
          </div>

          <!-- Live Summary Badge -->
          <div class="grid grid-cols-1 sm:grid-cols-2 gap-3 pt-1">
            <div class="bg-gradient-to-r from-emerald-950/60 via-slate-950 to-teal-950/60 border border-emerald-500/40 rounded-xl p-3 flex flex-col justify-between">
              <span class="text-[10px] text-emerald-400 font-mono font-semibold uppercase">Live Mean ESR (%) &plusmn; SD:</span>
              <span id="live-esr-mean-sd" class="text-lg font-mono font-bold text-emerald-300 mt-1">0.00% &plusmn; 0.00%</span>
            </div>

            <div class="bg-gradient-to-r from-indigo-950/60 via-slate-950 to-cyan-950/60 border border-cyan-500/40 rounded-xl p-3 flex flex-col justify-between">
              <span class="text-[10px] text-cyan-400 font-mono font-semibold uppercase">Live Mean EWC (%) &plusmn; SD:</span>
              <span id="live-ewc-mean-sd" class="text-lg font-mono font-bold text-cyan-300 mt-1">0.00% &plusmn; 0.00%</span>
            </div>
          </div>

          <div class="flex justify-end gap-2 pt-2">
            <button type="button" onclick="window.clearSampleInputs()" class="px-3 py-2 rounded-xl bg-slate-800 hover:bg-slate-700 text-slate-300 font-semibold transition-all">Clear</button>
            <button type="submit" class="px-5 py-2 rounded-xl bg-gradient-to-r from-teal-400 via-emerald-400 to-cyan-500 hover:from-teal-300 hover:to-cyan-400 text-slate-950 font-bold shadow-lg shadow-teal-500/25 transition-all flex items-center gap-1.5">
              <i data-lucide="plus-circle" class="w-4 h-4"></i> Save Hydrogel Group
            </button>
          </div>
        </form>

      </div>

      <!-- Logged Hydrogel Groups Table -->
      <div class="bg-slate-900 border border-slate-800 rounded-2xl p-5 shadow-xl space-y-3">
        
        <div class="flex items-center justify-between">
          <div class="flex items-center space-x-2">
            <i data-lucide="layers" class="w-4 h-4 text-teal-400"></i>
            <h3 class="text-sm font-bold text-slate-100">Logged Hydrogel Groups (Mean &plusmn; SD)</h3>
          </div>
          <span id="sample-table-badge" class="text-xs font-mono bg-teal-500/10 text-teal-300 px-2.5 py-0.5 rounded-full border border-teal-500/30">0 Groups</span>
        </div>

        <div class="overflow-x-auto rounded-xl border border-slate-800">
          <table class="w-full text-left text-xs text-slate-300">
            <thead class="bg-slate-950 text-slate-400 uppercase font-mono text-[10px] border-b border-slate-800">
              <tr>
                <th class="py-3 px-3">Group ID</th>
                <th class="py-3 px-3">Profile</th>
                <th class="py-3 px-3">Medium</th>
                <th class="py-3 px-3">N</th>
                <th class="py-3 px-3 font-mono">Mean W<sub>d</sub> (mg)</th>
                <th class="py-3 px-3 font-mono">Mean W<sub>s</sub> (mg)</th>
                <th class="py-3 px-3 font-mono text-teal-400">ESR (%) &plusmn; SD</th>
                <th class="py-3 px-3 font-mono text-cyan-300">EWC (%) &plusmn; SD</th>
                <th class="py-3 px-3 text-right">Actions</th>
              </tr>
            </thead>
            <tbody id="samples-table-body" class="divide-y divide-slate-800/60 bg-slate-900/50">
              <!-- Populated via JS -->
            </tbody>
          </table>
        </div>

      </div>

      <!-- DYNAMIC THERMO-RESPONSIVE PROFILE CANVAS (25°C to 100°C) -->
      <div class="bg-slate-900 border border-slate-800 rounded-2xl p-5 shadow-xl space-y-4">
        <div class="flex flex-wrap items-center justify-between gap-3 border-b border-slate-800 pb-3">
          <div class="flex items-center space-x-2">
            <div class="w-7 h-7 rounded-lg bg-rose-500/10 border border-rose-500/30 flex items-center justify-center">
              <i data-lucide="thermometer-snowflake" class="w-4 h-4 text-rose-400"></i>
            </div>
            <div>
              <h3 class="text-sm font-bold text-slate-100">Automated Thermo-Responsive Profile ($25\,^{\circ}\text{C} - 100\,^{\circ}\text{C}$)</h3>
              <p class="text-[11px] text-slate-400">Automated LCST / UCST Boltzmann Swelling Curve Modeling with SD Error Bars</p>
            </div>
          </div>

          <div class="flex items-center gap-2">
            <div class="flex items-center bg-slate-950 p-1 rounded-xl border border-slate-800 text-xs font-mono">
              <button id="temp-view-btn-seq" onclick="window.setTempChartMode('seq')" class="px-2.5 py-1 rounded-lg bg-teal-500/20 text-teal-300 font-bold border border-teal-500/30 transition-all">
                $S_{eq}$ Swelling %
              </button>
              <button id="temp-view-btn-ewc" onclick="window.setTempChartMode('ewc')" class="px-2.5 py-1 rounded-lg text-slate-400 hover:text-slate-200 transition-all">
                $EWC$ Water %
              </button>
              <button id="temp-view-btn-chi" onclick="window.setTempChartMode('chi')" class="px-2.5 py-1 rounded-lg text-slate-400 hover:text-slate-200 transition-all">
                Flory $\chi(T)$
              </button>
            </div>
          </div>
        </div>

        <div class="relative w-full h-80 bg-slate-950 rounded-xl border border-slate-800/80 p-2">
          <canvas id="temp-profile-canvas" class="w-full h-full block"></canvas>
        </div>

        <!-- Thermo-responsive profile metrics summary grid -->
        <div id="thermo-summary-cards" class="grid grid-cols-1 sm:grid-cols-3 gap-3 text-xs">
          <!-- Populated via JS -->
        </div>
      </div>

      <!-- Time-Point Replicate Kinetics Logger -->
      <div class="bg-slate-900 border border-slate-800 rounded-2xl p-5 shadow-xl space-y-4">
        
        <div class="flex flex-wrap items-center justify-between gap-3 border-b border-slate-800 pb-3">
          <div class="flex items-center space-x-2">
            <div class="w-7 h-7 rounded-lg bg-amber-500/10 border border-amber-500/30 flex items-center justify-center">
              <i data-lucide="timer" class="w-4 h-4 text-amber-400"></i>
            </div>
            <div>
              <h3 class="text-sm font-bold text-slate-100">Time-Point Replicate Kinetics Logger (Mean &plusmn; SD)</h3>
              <p class="text-[11px] text-slate-400">Log time-point masses across replicates to compute kinetic SD error bars</p>
            </div>
          </div>

          <div class="flex items-center gap-2">
            <label class="text-xs text-slate-400 font-medium">Active Group:</label>
            <select id="time-series-sample-select" onchange="window.switchTimeSeriesSample()" class="bg-slate-950 border border-slate-800 rounded-xl px-3 py-1.5 text-xs text-teal-300 font-mono focus:outline-none focus:border-teal-500">
              <!-- Populated via JS -->
            </select>
          </div>
        </div>

        <div id="time-series-summary-banner" class="bg-slate-950/80 border border-slate-800/80 rounded-xl p-3 flex flex-wrap items-center justify-between gap-3 text-xs font-mono">
          <div class="flex items-center gap-4">
            <div>
              <span class="text-slate-500 block text-[10px] uppercase">Mean W<sub>d</sub></span>
              <span id="ts-banner-wd" class="text-slate-200 font-bold">0.00 mg</span>
            </div>
            <div class="border-l border-slate-800 pl-4">
              <span class="text-slate-500 block text-[10px] uppercase">Max W<sub>t</sub> Recorded</span>
              <span id="ts-banner-max-wt" class="text-cyan-400 font-bold">0.00 mg</span>
            </div>
            <div class="border-l border-slate-800 pl-4">
              <span class="text-slate-500 block text-[10px] uppercase">Mean Swelling S<sub>t</sub> (%)</span>
              <span id="ts-banner-current-st" class="text-teal-400 font-bold">0.00% &plusmn; 0.00%</span>
            </div>
            <div class="border-l border-slate-800 pl-4">
              <span class="text-slate-500 block text-[10px] uppercase">Mean EWC<sub>t</sub> (%)</span>
              <span id="ts-banner-current-ewc" class="text-cyan-300 font-bold">0.00% &plusmn; 0.00%</span>
            </div>
          </div>

          <div class="flex items-center gap-2">
            <span id="ts-banner-progress" class="text-[10px] bg-amber-500/10 text-amber-400 border border-amber-500/20 px-2 py-0.5 rounded-full">0 / 7 Intervals Logged</span>
            <button onclick="window.autoApply24hAsWs()" class="px-2.5 py-1 text-[11px] bg-teal-500/20 hover:bg-teal-500/30 text-teal-300 rounded-lg border border-teal-500/30 transition-all">
              Apply 24h as W<sub>s</sub>
            </button>
          </div>
        </div>

        <div id="time-points-grid" class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-3">
          <!-- Populated via JS -->
        </div>

      </div>

      <!-- Kinetics Chart Section -->
      <div class="bg-slate-900 border border-slate-800 rounded-2xl p-5 shadow-xl space-y-3">
        <div class="flex flex-wrap items-center justify-between gap-2">
          <div class="flex items-center space-x-2">
            <i data-lucide="line-chart" class="w-4 h-4 text-cyan-400"></i>
            <h3 id="chart-title" class="text-sm font-bold text-slate-100">Kinetics Curve with Standard Deviation Error Bars</h3>
          </div>
          
          <div class="flex flex-wrap items-center gap-2">
            <div class="flex items-center bg-slate-950 p-1 rounded-xl border border-slate-800 text-xs font-mono">
              <button id="chart-btn-both" onclick="window.setChartMetric('both')" class="px-2.5 py-1 rounded-lg bg-gradient-to-r from-teal-500/20 to-fuchsia-500/20 text-teal-300 font-bold border border-teal-500/30 transition-all flex items-center gap-1">
                <i data-lucide="layers" class="w-3 h-3 text-teal-400"></i> Both Curves (Dual Axis)
              </button>
              <button id="chart-btn-st" onclick="window.setChartMetric('st')" class="px-2.5 py-1 rounded-lg text-slate-400 hover:text-slate-200 transition-all">
                S<sub>t</sub> Only
              </button>
              <button id="chart-btn-ewc" onclick="window.setChartMetric('ewc')" class="px-2.5 py-1 rounded-lg text-slate-400 hover:text-slate-200 transition-all">
                EWC<sub>t</sub> Only
              </button>
            </div>
          </div>
        </div>

        <div class="relative w-full h-80 bg-slate-950 rounded-xl border border-slate-800/80 p-2 flex items-center justify-center">
          <canvas id="kinetics-canvas" class="w-full h-full block"></canvas>
        </div>
      </div>

    </section>

  </main>

  <!-- MODALS -->
  <div id="modal-add-task" class="fixed inset-0 bg-slate-950/80 backdrop-blur-sm z-50 flex items-center justify-center p-4 hidden">
    <div class="bg-slate-900 border border-slate-800 rounded-2xl max-w-md w-full p-5 space-y-4 shadow-2xl">
      <div class="flex items-center justify-between border-b border-slate-800 pb-3">
        <h3 class="font-bold text-sm text-slate-100 flex items-center gap-2">
          <i data-lucide="plus-square" class="w-4 h-4 text-teal-400"></i> Add Experimental Task
        </h3>
        <button onclick="window.closeAddTaskModal()" class="text-slate-400 hover:text-white"><i data-lucide="x" class="w-4 h-4"></i></button>
      </div>

      <div class="space-y-3 text-xs">
        <div>
          <label class="block text-slate-400 font-medium mb-1">Task Title *</label>
          <input type="text" id="modal-task-title" required placeholder="e.g. Prepare 10mM UV Photoinitiator solution" class="w-full bg-slate-950 border border-slate-800 rounded-xl px-3 py-2 text-slate-100 focus:outline-none focus:border-teal-500">
        </div>

        <div>
          <label class="block text-slate-400 font-medium mb-1">Protocol Category</label>
          <select id="modal-task-category" class="w-full bg-slate-950 border border-slate-800 rounded-xl px-3 py-2 text-slate-100 focus:outline-none focus:border-teal-500">
            <option value="prep">1. Synthesis & Purification</option>
            <option value="weighing">2. Dry Weighing & Swelling Medium</option>
            <option value="kinetics">3. Kinetic Measurements & Math</option>
          </select>
        </div>

        <div>
          <label class="block text-slate-400 font-medium mb-1">Priority / Importance</label>
          <select id="modal-task-priority" class="w-full bg-slate-950 border border-slate-800 rounded-xl px-3 py-2 text-slate-100 focus:outline-none focus:border-teal-500">
            <option value="normal">Normal</option>
            <option value="high">Critical Step</option>
          </select>
        </div>
      </div>

      <div class="flex justify-end space-x-2 pt-2 border-t border-slate-800">
        <button onclick="window.closeAddTaskModal()" class="px-3 py-2 text-xs text-slate-300 hover:text-white">Cancel</button>
        <button onclick="window.saveNewTask()" class="px-4 py-2 bg-teal-500 hover:bg-teal-400 text-slate-950 font-bold rounded-xl text-xs shadow-lg shadow-teal-500/20">Save Task</button>
      </div>
    </div>
  </div>

  <div id="modal-export" class="fixed inset-0 bg-slate-950/80 backdrop-blur-sm z-50 flex items-center justify-center p-4 hidden">
    <div class="bg-slate-900 border border-slate-800 rounded-2xl max-w-lg w-full p-5 space-y-4 shadow-2xl">
      <div class="flex items-center justify-between border-b border-slate-800 pb-3">
        <h3 class="font-bold text-sm text-slate-100 flex items-center gap-2">
          <i data-lucide="file-spreadsheet" class="w-4 h-4 text-teal-400"></i> Export Statistical & Thermo Profile Data
        </h3>
        <button onclick="window.closeExportModal()" class="text-slate-400 hover:text-white"><i data-lucide="x" class="w-4 h-4"></i></button>
      </div>

      <p class="text-xs text-slate-400">Copy or download your hydrogel swelling dataset formatted for GraphPad Prism, Excel, or R (includes LCST/UCST profiles & $25\,^{\circ}\text{C}-100\,^{\circ}\text{C}$ matrix).</p>

      <textarea id="export-textarea" readonly class="w-full h-48 bg-slate-950 border border-slate-800 rounded-xl p-3 font-mono text-[11px] text-teal-300 focus:outline-none"></textarea>

      <div class="flex justify-between items-center pt-2 border-t border-slate-800 text-xs">
        <span class="text-slate-500">Formatted for GraphPad Prism / Excel</span>
        <div class="flex space-x-2">
          <button onclick="window.closeExportModal()" class="px-3 py-2 text-slate-300 hover:text-white">Close</button>
          <button onclick="window.copyExportToClipboard()" class="px-4 py-2 bg-teal-500 hover:bg-teal-400 text-slate-950 font-bold rounded-xl shadow-lg shadow-teal-500/20 flex items-center gap-1.5">
            <i data-lucide="copy" class="w-3.5 h-3.5"></i> Copy CSV
          </button>
        </div>
      </div>
    </div>
  </div>

  <!-- Notification Toast -->
  <div id="toast" class="fixed bottom-6 right-6 z-50 bg-teal-500 text-slate-950 font-bold text-xs px-4 py-3 rounded-xl shadow-2xl flex items-center gap-2 translate-y-20 opacity-0 transition-all duration-300 pointer-events-none">
    <i data-lucide="check-circle" class="w-4 h-4"></i>
    <span id="toast-text">Notification message</span>
  </div>

  <script>
    const TEMPERATURE_SERIES = [25, 30, 32.5, 35, 40, 45, 50, 55, 60, 70, 80, 90, 100];

    const TIME_INTERVALS = [
      { key: '30m', t: 0.5, label: '30 min' },
      { key: '1h',  t: 1.0, label: '1 hour' },
      { key: '2h',  t: 2.0, label: '2 hours' },
      { key: '3h',  t: 3.0, label: '3 hours' },
      { key: '4h',  t: 4.0, label: '4 hours' },
      { key: '8h',  t: 8.0, label: '8 hours' },
      { key: '24h', t: 24.0, label: '24 hours (Eq)' }
    ];

    const STANDARD_PROTOCOL_TASKS = [
      { id: 't1', title: 'Synthesize Polymer Precursor Hydrogel Solution', category: 'prep', priority: 'high', completed: true },
      { id: 't2', title: 'Prepare Swelling Buffer (PBS pH 7.4)', category: 'prep', priority: 'normal', completed: true },
      { id: 't3', title: 'Cast Hydrogel Disks into Mold (N=3 or N=5)', category: 'prep', priority: 'high', completed: true },
      { id: 't4', title: 'Crosslink under UV / Thermal Initiator', category: 'prep', priority: 'normal', completed: true },
      { id: 't5', title: 'Desiccate and Record Dry Mass (Wd) for all Replicates', category: 'weighing', priority: 'high', completed: false },
      { id: 't6', title: 'Immerse Samples in Swelling Medium at Target Temp', category: 'weighing', priority: 'normal', completed: false },
      { id: 't7', title: 'Log Swollen Mass (Wt) at Regular Kinetic Intervals', category: 'kinetics', priority: 'high', completed: false },
      { id: 't8', title: 'Record Equilibrium Swollen Mass (Ws) at 24h', category: 'kinetics', priority: 'high', completed: false },
      { id: 't9', title: 'Calculate Mean & SD for ESR and EWC across Temps', category: 'kinetics', priority: 'normal', completed: false }
    ];

    const INITIAL_SAMPLES = [
      { 
        id: 'GEL-PNIPAM-LCST', 
        composition: 'PNIPAM Hydrogel (Thermo-Responsive)', 
        medium: 'PBS pH 7.4', 
        temperature: 25,
        responsiveType: 'lcst',
        transitionTemp: 32.5,
        replicates: [
          { wd: 50.0, ws: 1100.0 },
          { wd: 49.5, ws: 1089.0 },
          { wd: 51.2, ws: 1126.0 }
        ],
        timeSeries: {
          '30m': [300.0, 297.0, 307.0],
          '1h':  [600.0, 594.0, 614.0],
          '2h':  [850.0, 841.0, 870.0],
          '3h':  [980.0, 970.0, 1003.0],
          '4h':  [1040.0, 1029.0, 1064.0],
          '8h':  [1085.0, 1074.0, 1111.0],
          '24h': [1100.0, 1089.0, 1126.0]
        }
      },
      { 
        id: 'GEL-PAAm-AAc-UCST', 
        composition: 'Poly(acrylamide-co-acrylic acid) IPN', 
        medium: 'Deionized Water', 
        temperature: 25,
        responsiveType: 'ucst',
        transitionTemp: 45.0,
        replicates: [
          { wd: 40.0, ws: 240.0 },
          { wd: 41.0, ws: 246.0 },
          { wd: 39.5, ws: 237.0 }
        ],
        timeSeries: {
          '30m': [80.0, 82.0, 79.0],
          '1h':  [130.0, 133.0, 128.0],
          '2h':  [180.0, 184.0, 178.0],
          '3h':  [210.0, 215.0, 207.0],
          '4h':  [225.0, 230.0, 222.0],
          '8h':  [238.0, 244.0, 235.0],
          '24h': [240.0, 246.0, 237.0]
        }
      },
      { 
        id: 'GEL-PEGDA-10%', 
        composition: 'PEGDA 10 wt% (Non-Responsive)', 
        medium: 'PBS pH 7.4',
        temperature: 37,
        responsiveType: 'non-responsive',
        transitionTemp: null,
        replicates: [
          { wd: 45.0, ws: 675.0 },
          { wd: 46.2, ws: 681.0 },
          { wd: 44.1, ws: 668.0 }
        ],
        timeSeries: {
          '30m': [225.0, 228.0, 220.0],
          '1h':  [382.5, 388.0, 375.0],
          '2h':  [517.5, 524.0, 510.0],
          '3h':  [585.0, 592.0, 578.0],
          '4h':  [630.0, 638.0, 622.0],
          '8h':  [666.0, 673.0, 658.0],
          '24h': [675.0, 681.0, 668.0]
        }
      }
    ];

    /* GLOBAL STATE */
    let appTasks = JSON.parse(JSON.stringify(STANDARD_PROTOCOL_TASKS));
    let appSamples = JSON.parse(JSON.stringify(INITIAL_SAMPLES));
    let activeCategoryFilter = 'all';
    let selectedTimeSeriesSampleId = appSamples[0] ? appSamples[0].id : '';
    let chartMetric = 'both'; // 'both', 'st', or 'ewc'
    let tempChartMode = 'seq'; // 'seq', 'ewc', or 'chi'

    function calcMean(arr) {
      if (!arr || arr.length === 0) return 0;
      const sum = arr.reduce((a, b) => a + b, 0);
      return sum / arr.length;
    }

    function calcSD(arr) {
      if (!arr || arr.length <= 1) return 0;
      const mean = calcMean(arr);
      const variance = arr.reduce((a, b) => a + Math.pow(b - mean, 2), 0) / (arr.length - 1);
      return Math.sqrt(variance);
    }

    function escapeHtml(str) {
      if (typeof str !== 'string') return '';
      return str.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;").replace(/"/g, "&quot;").replace(/'/g, "&#039;");
    }

    function computeSampleStats(sample) {
      if (!sample || !sample.replicates || sample.replicates.length === 0) {
        return { meanWd: 0, sdWd: 0, meanWs: 0, sdWs: 0, meanEsr: 0, sdEsr: 0, meanEwc: 0, sdEwc: 0 };
      }
      const wds = sample.replicates.map(r => r.wd || 0);
      const wss = sample.replicates.map(r => r.ws || 0);
      const esrs = sample.replicates.map(r => (r.ws && r.wd && r.wd > 0) ? ((r.ws - r.wd) / r.wd) * 100 : 0);
      const ewcs = sample.replicates.map(r => (r.ws && r.wd && r.ws > 0) ? ((r.ws - r.wd) / r.ws) * 100 : 0);

      return {
        meanWd: calcMean(wds),
        sdWd: calcSD(wds),
        meanWs: calcMean(wss),
        sdWs: calcSD(wss),
        meanEsr: calcMean(esrs),
        sdEsr: calcSD(esrs),
        meanEwc: calcMean(ewcs),
        sdEwc: calcSD(ewcs)
      };
    }

    /**
     * Compute Thermo-Responsive Equilibrium Swelling Profile S_eq(T) and EWC(T) across 25°C to 100°C
     * Based on Sigmoidal Boltzmann Phase Transition kinetics & Flory-Huggins thermodynamic parameter chi(T).
     */
    function computeThermoProfileForSample(sample) {
      const stats = computeSampleStats(sample);
      const baseEsr = stats.meanEsr;
      const baseSdEsr = stats.sdEsr;
      const type = sample.responsiveType || 'non-responsive';
      const Tc = sample.transitionTemp || (type === 'lcst' ? 32.5 : (type === 'ucst' ? 45.0 : 37.0));

      const profile = TEMPERATURE_SERIES.map(temp => {
        let Seq = baseEsr;
        let sdSeq = baseSdEsr;
        let deltaT = 2.5; // Transition steepness width (°C)

        if (type === 'lcst') {
          // LCST Boltzmann transition: High swelling at low T, sudden sharp collapse above Tc
          // Min collapsed swelling is ~ 10-15% of max room-temp swollen ratio
          const minSwell = Math.max(150.0, baseEsr * 0.12);
          const maxSwell = baseEsr;
          Seq = minSwell + (maxSwell - minSwell) / (1 + Math.exp((temp - Tc) / deltaT));
          sdSeq = baseSdEsr * (Seq / baseEsr);
        } else if (type === 'ucst') {
          // UCST Boltzmann transition: Low collapsed swelling below Tc, high swelling above Tc
          // Max high-temp swollen ratio is ~ 4-5x room temp collapsed ratio
          const minSwell = baseEsr;
          const maxSwell = Math.max(baseEsr * 4.2, 1800.0);
          Seq = minSwell + (maxSwell - minSwell) / (1 + Math.exp((Tc - temp) / deltaT));
          sdSeq = baseSdEsr * (Seq / baseEsr);
        } else {
          // Non-responsive / ideal hydrogel (e.g. PEGDA): Slight thermal contraction (~ -0.15%/°C)
          const thermalCoeff = -0.0015;
          Seq = baseEsr * (1 + thermalCoeff * (temp - 25));
          sdSeq = baseSdEsr;
        }

        const ewc = (Seq / (Seq + 100)) * 100;
        const sdEwc = (sdSeq / Seq) * ewc * 0.15; // Proportional error propagation

        // Estimate Flory-Huggins Interaction Parameter chi(T)
        // At theta point / phase boundary (Tc), chi = 0.5. Below LCST, chi < 0.5 (miscible). Above LCST, chi > 0.5 (phase separation).
        let chi = 0.5;
        if (type === 'lcst') {
          chi = 0.5 + 0.25 * (1 / (1 + Math.exp((Tc - temp) / 3.0)) - 0.5);
        } else if (type === 'ucst') {
          chi = 0.5 + 0.25 * (1 / (1 + Math.exp((temp - Tc) / 3.0)) - 0.5);
        } else {
          chi = 0.42 + 0.0003 * (temp - 25);
        }

        return { temp, Seq, sdSeq, ewc, sdEwc, chi };
      });

      return { sampleId: sample.id, responsiveType: type, transitionTemp: Tc, profile };
    }

    function refreshIcons() {
      if (window.lucide) {
        window.lucide.createIcons();
      }
    }

    function renderTasks() {
      const container = document.getElementById('tasks-list');
      if (!container) return;

      const filtered = appTasks.filter(t => {
        if (activeCategoryFilter === 'all') return true;
        return t.category === activeCategoryFilter;
      });

      const completedCount = appTasks.filter(t => t.completed).length;
      const totalCount = appTasks.length;
      const pct = totalCount > 0 ? Math.round((completedCount / totalCount) * 100) : 0;

      const progressPercentEl = document.getElementById('task-progress-percent');
      const progressBarEl = document.getElementById('task-progress-bar');
      const headerProgressEl = document.getElementById('header-progress-text');

      if (progressPercentEl) progressPercentEl.innerText = `${pct}%`;
      if (progressBarEl) progressBarEl.style.width = `${pct}%`;
      if (headerProgressEl) headerProgressEl.innerText = `${completedCount} / ${totalCount} Done`;

      if (filtered.length === 0) {
        container.innerHTML = `<div class="p-4 text-center text-slate-500 text-xs font-mono">No tasks found for this category filter.</div>`;
        return;
      }

      container.innerHTML = filtered.map(t => {
        const isDone = t.completed;
        return `
          <div data-task-id="${escapeHtml(t.id)}" class="flex items-center justify-between p-2.5 rounded-xl border transition-all cursor-pointer ${
            isDone 
              ? 'bg-slate-950/40 border-slate-800/60 opacity-60' 
              : 'bg-slate-900/90 border-slate-800 hover:border-slate-700'
          }">
            <div class="flex items-center space-x-2.5 min-w-0">
              <div class="w-5 h-5 rounded-md border flex items-center justify-center transition-colors ${
                isDone ? 'bg-emerald-500 border-emerald-400 text-slate-950' : 'border-slate-700 bg-slate-950/60'
              }">
                ${isDone ? '<i data-lucide="check" class="w-3.5 h-3.5 stroke-[3]"></i>' : ''}
              </div>
              <span class="text-xs ${isDone ? 'line-through text-slate-500' : 'text-slate-200 font-medium'} truncate">
                ${escapeHtml(t.title)}
              </span>
            </div>
            ${t.priority === 'high' ? '<span class="text-[9px] font-mono font-semibold px-1.5 py-0.5 rounded bg-rose-500/10 text-rose-400 border border-rose-500/30">Critical</span>' : ''}
          </div>
        `;
      }).join('');

      refreshIcons();
    }

    function toggleTask(id) {
      const task = appTasks.find(t => t.id === id);
      if (task) {
        task.completed = !task.completed;
        renderTasks();
      }
    }

    function filterTasks(category) {
      activeCategoryFilter = category;
      ['all', 'prep', 'weighing', 'kinetics'].forEach(cat => {
        const btn = document.getElementById(`task-tab-${cat}`);
        if (btn) {
          if (cat === category) {
            btn.className = 'px-3 py-1 rounded-lg bg-teal-500/20 text-teal-300 font-semibold border border-teal-500/30 whitespace-nowrap';
          } else {
            btn.className = 'px-3 py-1 rounded-lg hover:text-slate-200 whitespace-nowrap border border-transparent';
          }
        }
      });
      renderTasks();
    }

    function openAddTaskModal() {
      const modal = document.getElementById('modal-add-task');
      if (modal) modal.classList.remove('hidden');
    }

    function closeAddTaskModal() {
      const modal = document.getElementById('modal-add-task');
      if (modal) modal.classList.add('hidden');
    }

    function saveNewTask() {
      const titleInput = document.getElementById('modal-task-title');
      const catInput = document.getElementById('modal-task-category');
      const priorityInput = document.getElementById('modal-task-priority');

      if (!titleInput || !titleInput.value.trim()) {
        showToast('Please enter a task title.');
        return;
      }

      const newTask = {
        id: 't_' + Date.now(),
        title: titleInput.value.trim(),
        category: catInput ? catInput.value : 'prep',
        priority: priorityInput ? priorityInput.value : 'normal',
        completed: false
      };

      appTasks.push(newTask);
      titleInput.value = '';
      closeAddTaskModal();
      renderTasks();
      showToast('Task added to protocol list.');
    }

    function clearCompletedTasks() {
      appTasks = appTasks.filter(t => !t.completed);
      renderTasks();
      showToast('Cleared completed tasks.');
    }

    function loadStandardProtocol() {
      appTasks = JSON.parse(JSON.stringify(STANDARD_PROTOCOL_TASKS));
      renderTasks();
      showToast('Reset standard protocol task list.');
    }

    function handleResponsiveTypeChange() {
      const typeSelect = document.getElementById('input-responsive-type');
      const tcInput = document.getElementById('input-transition-temp');
      if (!typeSelect || !tcInput) return;

      if (typeSelect.value === 'lcst') {
        tcInput.value = "32.5";
      } else if (typeSelect.value === 'ucst') {
        tcInput.value = "45.0";
      } else {
        tcInput.value = "";
      }
    }

    function renderReplicateInputs() {
      const select = document.getElementById('input-replicate-count');
      const container = document.getElementById('replicates-container');
      if (!select || !container) return;

      const count = parseInt(select.value) || 1;
      let html = '';

      for (let i = 0; i < count; i++) {
        html += `
          <div class="grid grid-cols-5 gap-2 items-center text-xs">
            <span class="font-mono text-slate-400 font-semibold text-[11px]">R#${i + 1}</span>
            <input type="number" step="0.01" min="0.01" id="rep-wd-${i}" oninput="window.liveCalculateESR()" placeholder="Wd (mg)" required class="bg-slate-900 border border-slate-800 rounded-lg px-2.5 py-1 text-slate-100 font-mono focus:outline-none focus:border-teal-400">
            <input type="number" step="0.01" min="0.01" id="rep-ws-${i}" oninput="window.liveCalculateESR()" placeholder="Ws (mg)" required class="bg-slate-900 border border-slate-800 rounded-lg px-2.5 py-1 text-slate-100 font-mono focus:outline-none focus:border-teal-400">
            <span id="rep-esr-calc-${i}" class="font-mono font-bold text-teal-400 text-right pr-2">0.00%</span>
            <span id="rep-ewc-calc-${i}" class="font-mono font-bold text-cyan-300 text-right pr-2">0.00%</span>
          </div>
        `;
      }

      container.innerHTML = html;
      liveCalculateESR();
    }

    function liveCalculateESR() {
      const select = document.getElementById('input-replicate-count');
      if (!select) return;
      const count = parseInt(select.value) || 1;

      const esrs = [];
      const ewcs = [];

      for (let i = 0; i < count; i++) {
        const wdEl = document.getElementById(`rep-wd-${i}`);
        const wsEl = document.getElementById(`rep-ws-${i}`);
        const esrCalcEl = document.getElementById(`rep-esr-calc-${i}`);
        const ewcCalcEl = document.getElementById(`rep-ewc-calc-${i}`);

        const wd = wdEl ? parseFloat(wdEl.value) : NaN;
        const ws = wsEl ? parseFloat(wsEl.value) : NaN;

        if (!isNaN(wd) && !isNaN(ws) && wd > 0 && ws >= wd) {
          const esr = ((ws - wd) / wd) * 100;
          const ewc = ((ws - wd) / ws) * 100;
          esrs.push(esr);
          ewcs.push(ewc);

          if (esrCalcEl) esrCalcEl.innerText = `${esr.toFixed(1)}%`;
          if (ewcCalcEl) ewcCalcEl.innerText = `${ewc.toFixed(1)}%`;
        } else {
          if (esrCalcEl) esrCalcEl.innerText = '0.00%';
          if (ewcCalcEl) ewcCalcEl.innerText = '0.00%';
        }
      }

      const meanEsrEl = document.getElementById('live-esr-mean-sd');
      const meanEwcEl = document.getElementById('live-ewc-mean-sd');

      if (esrs.length > 0) {
        const mEsr = calcMean(esrs);
        const sdEsr = calcSD(esrs);
        const mEwc = calcMean(ewcs);
        const sdEwc = calcSD(ewcs);

        if (meanEsrEl) meanEsrEl.innerText = `${mEsr.toFixed(2)}% \u00B1 ${sdEsr.toFixed(2)}%`;
        if (meanEwcEl) meanEwcEl.innerText = `${mEwc.toFixed(2)}% \u00B1 ${sdEwc.toFixed(2)}%`;
      } else {
        if (meanEsrEl) meanEsrEl.innerText = '0.00% \u00B1 0.00%';
        if (meanEwcEl) meanEwcEl.innerText = '0.00% \u00B1 0.00%';
      }
    }

    function handleSampleSubmit(event) {
      event.preventDefault();
      const sampleIdInput = document.getElementById('input-sample-id');
      const compInput = document.getElementById('input-composition');
      const mediumInput = document.getElementById('input-medium');
      const selectCount = document.getElementById('input-replicate-count');
      const typeSelect = document.getElementById('input-responsive-type');
      const tcInput = document.getElementById('input-transition-temp');

      if (!sampleIdInput || !sampleIdInput.value.trim()) {
        showToast('Please enter a Group / Sample ID.');
        return;
      }

      const count = selectCount ? parseInt(selectCount.value) || 1 : 1;
      const replicates = [];

      for (let i = 0; i < count; i++) {
        const wd = parseFloat(document.getElementById(`rep-wd-${i}`)?.value);
        const ws = parseFloat(document.getElementById(`rep-ws-${i}`)?.value);

        if (isNaN(wd) || isNaN(ws) || wd <= 0 || ws < wd) {
          showToast(`Please enter valid Wd and Ws for Replicate #${i + 1}`);
          return;
        }
        replicates.push({ wd, ws });
      }

      const sampleId = sampleIdInput.value.trim();
      const existingIdx = appSamples.findIndex(s => s.id === sampleId);

      const respType = typeSelect ? typeSelect.value : 'non-responsive';
      const tcVal = tcInput && tcInput.value ? parseFloat(tcInput.value) : (respType === 'lcst' ? 32.5 : (respType === 'ucst' ? 45.0 : null));

      const newGroup = {
        id: sampleId,
        composition: compInput ? compInput.value.trim() || 'Hydrogel Formulation' : 'Hydrogel Formulation',
        medium: mediumInput ? mediumInput.value.trim() || 'PBS pH 7.4' : 'PBS pH 7.4',
        temperature: 25,
        responsiveType: respType,
        transitionTemp: tcVal,
        replicates,
        timeSeries: existingIdx >= 0 ? appSamples[existingIdx].timeSeries : {}
      };

      if (existingIdx >= 0) {
        appSamples[existingIdx] = newGroup;
        showToast(`Updated Hydrogel Group: ${sampleId}`);
      } else {
        appSamples.push(newGroup);
        showToast(`Saved Hydrogel Group: ${sampleId}`);
      }

      selectedTimeSeriesSampleId = sampleId;
      clearSampleInputs();
      renderSamples();
      populateTimeSeriesSelect();
      renderTimeSeriesGrid();
      drawKineticsChart();
      drawTemperatureProfileChart();
    }

    function clearSampleInputs() {
      const form = document.getElementById('sample-form');
      if (form) form.reset();
      renderReplicateInputs();
    }

    function renderSamples() {
      const tableBody = document.getElementById('samples-table-body');
      const countBadge = document.getElementById('sample-table-badge');
      const headerCount = document.getElementById('header-sample-count');
      const headerAvgEsr = document.getElementById('header-avg-esr');

      if (countBadge) countBadge.innerText = `${appSamples.length} Groups`;
      if (headerCount) headerCount.innerText = `${appSamples.length} Groups`;

      let overallEsrs = [];
      appSamples.forEach(s => {
        const stats = computeSampleStats(s);
        overallEsrs.push(stats.meanEsr);
      });

      if (headerAvgEsr) {
        if (overallEsrs.length > 0) {
          const avg = calcMean(overallEsrs);
          const sd = calcSD(overallEsrs);
          headerAvgEsr.innerText = `${avg.toFixed(1)}% \u00B1 ${sd.toFixed(1)}%`;
        } else {
          headerAvgEsr.innerText = '0.00%';
        }
      }

      if (!tableBody) return;

      if (appSamples.length === 0) {
        tableBody.innerHTML = `<tr><td colspan="9" class="py-6 text-center text-slate-500 font-mono text-xs">No hydrogel groups logged yet. Use the calculator above to log samples.</td></tr>`;
        return;
      }

      tableBody.innerHTML = appSamples.map(sample => {
        const stats = computeSampleStats(sample);
        const n = sample.replicates ? sample.replicates.length : 0;
        const type = sample.responsiveType || 'non-responsive';
        
        let typeBadge = '';
        if (type === 'lcst') {
          typeBadge = `<span class="text-[9px] font-mono font-bold px-2 py-0.5 rounded bg-teal-500/20 text-teal-300 border border-teal-500/40">LCST (${sample.transitionTemp || 32.5}°C)</span>`;
        } else if (type === 'ucst') {
          typeBadge = `<span class="text-[9px] font-mono font-bold px-2 py-0.5 rounded bg-amber-500/20 text-amber-300 border border-amber-500/40">UCST (${sample.transitionTemp || 45.0}°C)</span>`;
        } else {
          typeBadge = `<span class="text-[9px] font-mono font-bold px-2 py-0.5 rounded bg-slate-800 text-slate-400 border border-slate-700">Constant</span>`;
        }

        return `
          <tr class="hover:bg-slate-800/40 transition-colors">
            <td class="py-3 px-3 font-bold text-slate-100 font-mono">${escapeHtml(sample.id)}</td>
            <td class="py-3 px-3">${typeBadge}</td>
            <td class="py-3 px-3 text-slate-400 max-w-[120px] truncate">${escapeHtml(sample.medium)}</td>
            <td class="py-3 px-3 font-mono text-purple-300 font-bold">${n}</td>
            <td class="py-3 px-3 font-mono">${stats.meanWd.toFixed(1)} &plusmn; ${stats.sdWd.toFixed(1)}</td>
            <td class="py-3 px-3 font-mono">${stats.meanWs.toFixed(1)} &plusmn; ${stats.sdWs.toFixed(1)}</td>
            <td class="py-3 px-3 font-mono font-bold text-teal-400">${stats.meanEsr.toFixed(1)}% &plusmn; ${stats.sdEsr.toFixed(1)}%</td>
            <td class="py-3 px-3 font-mono font-bold text-cyan-300">${stats.meanEwc.toFixed(1)}% &plusmn; ${stats.sdEwc.toFixed(1)}%</td>
            <td class="py-3 px-3 text-right">
              <button data-delete-sample-id="${escapeHtml(sample.id)}" class="p-1 text-slate-500 hover:text-rose-400 transition-colors rounded-lg hover:bg-rose-500/10">
                <i data-lucide="trash-2" class="w-4 h-4"></i>
              </button>
            </td>
          </tr>
        `;
      }).join('');

      renderThermoSummaryCards();
      refreshIcons();
    }

    function renderThermoSummaryCards() {
      const container = document.getElementById('thermo-summary-cards');
      if (!container) return;

      if (appSamples.length === 0) {
        container.innerHTML = '<div class="col-span-full text-center text-slate-500 font-mono">No hydrogel sample profiles to display.</div>';
        return;
      }

      container.innerHTML = appSamples.slice(0, 3).map(sample => {
        const stats = computeSampleStats(sample);
        const profileData = computeThermoProfileForSample(sample);
        const type = sample.responsiveType || 'non-responsive';
        
        const swell25 = profileData.profile.find(p => p.temp === 25)?.Seq || stats.meanEsr;
        const swell50 = profileData.profile.find(p => p.temp === 50)?.Seq || stats.meanEsr;
        const swell100 = profileData.profile.find(p => p.temp === 100)?.Seq || stats.meanEsr;

        let statusText = '';
        if (type === 'lcst') statusText = 'Collapses above ' + (sample.transitionTemp || 32.5) + ' °C';
        else if (type === 'ucst') statusText = 'Swells above ' + (sample.transitionTemp || 45.0) + ' °C';
        else statusText = 'Thermo-Insensitive Network';

        return `
          <div class="bg-slate-950/80 border border-slate-800 rounded-xl p-3 flex flex-col justify-between space-y-2">
            <div>
              <div class="flex items-center justify-between">
                <span class="font-bold text-slate-100 truncate">${escapeHtml(sample.id)}</span>
                <span class="text-[9px] font-mono px-1.5 py-0.5 rounded ${type === 'lcst' ? 'bg-teal-500/20 text-teal-300' : (type === 'ucst' ? 'bg-amber-500/20 text-amber-300' : 'bg-slate-800 text-slate-400')}">
                  ${type.toUpperCase()}
                </span>
              </div>
              <span class="text-[10px] text-slate-400 block mt-0.5">${statusText}</span>
            </div>

            <div class="grid grid-cols-3 gap-1 text-[10px] font-mono bg-slate-900/80 p-2 rounded-lg text-center border border-slate-800/80">
              <div>
                <span class="text-slate-500 block text-[8.5px]">25 °C</span>
                <span class="text-teal-300 font-bold">${swell25.toFixed(0)}%</span>
              </div>
              <div class="border-x border-slate-800">
                <span class="text-slate-500 block text-[8.5px]">50 °C</span>
                <span class="text-amber-300 font-bold">${swell50.toFixed(0)}%</span>
              </div>
              <div>
                <span class="text-slate-500 block text-[8.5px]">100 °C</span>
                <span class="text-rose-400 font-bold">${swell100.toFixed(0)}%</span>
              </div>
            </div>
          </div>
        `;
      }).join('');
    }

    function deleteSample(sampleId) {
      appSamples = appSamples.filter(s => s.id !== sampleId);
      if (selectedTimeSeriesSampleId === sampleId) {
        selectedTimeSeriesSampleId = appSamples[0] ? appSamples[0].id : '';
      }
      renderSamples();
      populateTimeSeriesSelect();
      renderTimeSeriesGrid();
      drawKineticsChart();
      drawTemperatureProfileChart();
      showToast(`Deleted group: ${sampleId}`);
    }

    function populateTimeSeriesSelect() {
      const select = document.getElementById('time-series-sample-select');
      if (!select) return;

      if (appSamples.length === 0) {
        select.innerHTML = '<option value="">No Groups Logged</option>';
        return;
      }

      select.innerHTML = appSamples.map(s => `
        <option value="${escapeHtml(s.id)}" ${s.id === selectedTimeSeriesSampleId ? 'selected' : ''}>${escapeHtml(s.id)}</option>
      `).join('');
    }

    function switchTimeSeriesSample() {
      const select = document.getElementById('time-series-sample-select');
      if (select) {
        selectedTimeSeriesSampleId = select.value;
        renderTimeSeriesGrid();
        drawKineticsChart();
      }
    }

    function setChartMetric(metric) {
      chartMetric = metric;
      const btnBoth = document.getElementById('chart-btn-both');
      const btnSt = document.getElementById('chart-btn-st');
      const btnEwc = document.getElementById('chart-btn-ewc');

      const activeClass = 'px-2.5 py-1 rounded-lg bg-teal-500/20 text-teal-300 font-bold border border-teal-500/30 transition-all flex items-center gap-1';
      const inactiveClass = 'px-2.5 py-1 rounded-lg text-slate-400 hover:text-slate-200 transition-all';

      if (btnBoth) btnBoth.className = metric === 'both' ? activeClass : inactiveClass;
      if (btnSt) btnSt.className = metric === 'st' ? activeClass : inactiveClass;
      if (btnEwc) btnEwc.className = metric === 'ewc' ? activeClass : inactiveClass;

      drawKineticsChart();
    }

    function setTempChartMode(mode) {
      tempChartMode = mode;
      const btnSeq = document.getElementById('temp-view-btn-seq');
      const btnEwc = document.getElementById('temp-view-btn-ewc');
      const btnChi = document.getElementById('temp-view-btn-chi');

      const activeClass = 'px-2.5 py-1 rounded-lg bg-teal-500/20 text-teal-300 font-bold border border-teal-500/30 transition-all';
      const inactiveClass = 'px-2.5 py-1 rounded-lg text-slate-400 hover:text-slate-200 transition-all';

      if (btnSeq) btnSeq.className = mode === 'seq' ? activeClass : inactiveClass;
      if (btnEwc) btnEwc.className = mode === 'ewc' ? activeClass : inactiveClass;
      if (btnChi) btnChi.className = mode === 'chi' ? activeClass : inactiveClass;

      drawTemperatureProfileChart();
    }

    function renderTimeSeriesGrid() {
      const container = document.getElementById('time-points-grid');
      const sample = appSamples.find(s => s.id === selectedTimeSeriesSampleId);
      const bannerWd = document.getElementById('ts-banner-wd');
      const bannerMaxWt = document.getElementById('ts-banner-max-wt');
      const bannerCurrentSt = document.getElementById('ts-banner-current-st');
      const bannerCurrentEwc = document.getElementById('ts-banner-current-ewc');
      const bannerProgress = document.getElementById('ts-banner-progress');

      if (!container) return;

      if (!sample) {
        container.innerHTML = '<div class="col-span-full py-6 text-center text-slate-500 text-xs font-mono">Please log or select a hydrogel group to enter replicate time-point masses (Wt).</div>';
        if (bannerWd) bannerWd.innerText = '0.00 mg';
        if (bannerMaxWt) bannerMaxWt.innerText = '0.00 mg';
        if (bannerCurrentSt) bannerCurrentSt.innerText = '0.00%';
        if (bannerCurrentEwc) bannerCurrentEwc.innerText = '0.00%';
        if (bannerProgress) bannerProgress.innerText = '0 / 7 Logged';
        return;
      }

      if (!sample.timeSeries) sample.timeSeries = {};

      const stats = computeSampleStats(sample);
      const repCount = sample.replicates ? sample.replicates.length : 1;

      let loggedIntervalsCount = 0;
      let maxWtValue = 0;

      let html = '';
      TIME_INTERVALS.forEach(interval => {
        const key = interval.key;
        const weights = sample.timeSeries[key] || [];

        const isSet = weights.some(w => w !== undefined && w !== null && !isNaN(w) && w > 0);
        if (isSet) loggedIntervalsCount++;

        const stArr = [];
        const ewcArr = [];

        for (let rIdx = 0; rIdx < repCount; rIdx++) {
          const wt = weights[rIdx];
          const repWd = sample.replicates[rIdx] ? sample.replicates[rIdx].wd : stats.meanWd;

          if (wt && wt > maxWtValue) maxWtValue = wt;
          if (wt && repWd && wt > repWd) {
            stArr.push(((wt - repWd) / repWd) * 100);
            ewcArr.push(((wt - repWd) / wt) * 100);
          }
        }

        const meanSt = calcMean(stArr);
        const sdSt = calcSD(stArr);
        const meanEwc = calcMean(ewcArr);
        const sdEwc = calcSD(ewcArr);

        const escSampleId = escapeHtml(sample.id);
        const escKey = escapeHtml(key);

        html += '<div class="bg-slate-950/80 border ' + (isSet ? 'border-emerald-500/40 bg-gradient-to-b from-emerald-950/20 to-slate-950' : 'border-slate-800') + ' rounded-xl p-3 flex flex-col justify-between space-y-2.5 transition-all shadow-sm">';
        html += '  <div class="flex items-center justify-between border-b border-slate-800 pb-1.5">';
        html += '    <span class="font-mono text-xs font-bold ' + (isSet ? 'text-amber-400' : 'text-slate-400') + '">' + escapeHtml(interval.label) + '</span>';
        html += '    <span class="text-[9px] px-1.5 py-0.5 rounded ' + (isSet ? 'bg-emerald-500/20 text-emerald-300 font-bold border border-emerald-500/30' : 'bg-slate-800 text-slate-500') + '">';
        html += '      ' + (isSet ? 'Logged' : 'Pending');
        html += '    </span>';
        html += '  </div>';

        html += '  <div class="space-y-1.5">';
        for (let rIdx = 0; rIdx < repCount; rIdx++) {
          const val = weights[rIdx] !== undefined && weights[rIdx] !== null ? weights[rIdx] : '';
          html += '    <div class="flex items-center gap-2 text-[10px] font-mono">';
          html += '      <span class="text-slate-500 w-12">R#' + (rIdx + 1) + ':</span>';
          html += '      <input type="number" step="0.01" min="0" placeholder="0.00" ';
          html += '             value="' + val + '" ';
          html += '             data-sample-id="' + escSampleId + '" ';
          html += '             data-time-key="' + escKey + '" ';
          html += '             data-rep-idx="' + rIdx + '" ';
          html += '             class="w-full bg-slate-900 border border-slate-800 rounded px-2 py-1 text-xs text-slate-100 font-mono focus:outline-none focus:border-teal-400">';
          html += '    </div>';
        }
        html += '  </div>';

        html += '  <div class="pt-1 border-t border-slate-800/80 flex flex-col gap-0.5 text-[10px] font-mono">';
        html += '    <div class="flex items-center justify-between">';
        html += '      <span class="text-slate-500 text-[9px]">S<sub>t</sub> (&plusmn;SD):</span>';
        html += '      <span class="font-bold ' + (isSet ? 'text-emerald-400' : 'text-slate-600') + '">' + (isSet ? meanSt.toFixed(1) + '% \u00B1 ' + sdSt.toFixed(1) + '%' : '0.0%') + '</span>';
        html += '    </div>';
        html += '    <div class="flex items-center justify-between">';
        html += '      <span class="text-slate-500 text-[9px]">EWC<sub>t</sub> (&plusmn;SD):</span>';
        html += '      <span class="font-bold ' + (isSet ? 'text-cyan-300' : 'text-slate-600') + '">' + (isSet ? meanEwc.toFixed(1) + '% \u00B1 ' + sdEwc.toFixed(1) + '%' : '0.0%') + '</span>';
        html += '    </div>';
        html += '  </div>';
        html += '</div>';
      });

      container.innerHTML = html;

      const lastInterval = TIME_INTERVALS[TIME_INTERVALS.length - 1].key;
      const lastWeights = sample.timeSeries[lastInterval] || [];
      const stLast = [];
      const ewcLast = [];

      for (let rIdx = 0; rIdx < repCount; rIdx++) {
        const wt = lastWeights[rIdx];
        const repWd = sample.replicates[rIdx] ? sample.replicates[rIdx].wd : stats.meanWd;
        if (wt && repWd && wt > repWd) {
          stLast.push(((wt - repWd) / repWd) * 100);
          ewcLast.push(((wt - repWd) / wt) * 100);
        }
      }

      const meanStBanner = stLast.length > 0 ? calcMean(stLast) : stats.meanEsr;
      const sdStBanner = stLast.length > 0 ? calcSD(stLast) : stats.sdEsr;
      const meanEwcBanner = ewcLast.length > 0 ? calcMean(ewcLast) : stats.meanEwc;
      const sdEwcBanner = ewcLast.length > 0 ? calcSD(ewcLast) : stats.sdEwc;

      if (bannerWd) bannerWd.innerText = stats.meanWd.toFixed(2) + ' mg';
      if (bannerMaxWt) bannerMaxWt.innerText = maxWtValue.toFixed(2) + ' mg';
      if (bannerCurrentSt) bannerCurrentSt.innerText = meanStBanner.toFixed(1) + '% \u00B1 ' + sdStBanner.toFixed(1) + '%';
      if (bannerCurrentEwc) bannerCurrentEwc.innerText = meanEwcBanner.toFixed(1) + '% \u00B1 ' + sdEwcBanner.toFixed(1) + '%';
      if (bannerProgress) bannerProgress.innerText = loggedIntervalsCount + ' / 7 Intervals Logged';
    }

    function updateTimePointReplicateWeight(sampleId, timeKey, repIdx, valStr) {
      const sample = appSamples.find(s => s.id === sampleId);
      if (!sample) return;

      if (!sample.timeSeries) sample.timeSeries = {};
      if (!sample.timeSeries[timeKey]) sample.timeSeries[timeKey] = [];

      const val = parseFloat(valStr);
      sample.timeSeries[timeKey][repIdx] = (!isNaN(val) && val > 0) ? val : null;

      renderTimeSeriesGrid();
      drawKineticsChart();
    }

    function autoApply24hAsWs() {
      const sample = appSamples.find(s => s.id === selectedTimeSeriesSampleId);
      if (!sample || !sample.timeSeries || !sample.timeSeries['24h']) {
        showToast('Please log 24h replicate weights first.');
        return;
      }

      const logs24h = sample.timeSeries['24h'];
      sample.replicates.forEach((r, idx) => {
        if (logs24h[idx] && logs24h[idx] > r.wd) {
          r.ws = logs24h[idx];
        }
      });

      renderSamples();
      renderTimeSeriesGrid();
      drawKineticsChart();
      drawTemperatureProfileChart();
      showToast('Applied 24h replicate weights as equilibrium Ws.');
    }

    function drawKineticsChart() {
      const canvas = document.getElementById('kinetics-canvas');
      if (!canvas) return;
      const ctx = canvas.getContext('2d');
      if (!ctx) return;
      
      const rect = canvas.getBoundingClientRect();
      const dpr = window.devicePixelRatio || 1;
      canvas.width = rect.width * dpr;
      canvas.height = rect.height * dpr;
      ctx.scale(dpr, dpr);

      const width = rect.width;
      const height = rect.height;

      ctx.clearRect(0, 0, width, height);

      const isDual = chartMetric === 'both';
      const paddingLeft = 55;
      const paddingRight = isDual ? 55 : 35;
      const paddingTop = 40;
      const paddingBottom = 40;

      const plotWidth = width - paddingLeft - paddingRight;
      const plotHeight = height - paddingTop - paddingBottom;

      // Draw Grid
      ctx.strokeStyle = '#1e293b';
      ctx.lineWidth = 1;
      for (let i = 0; i <= 4; i++) {
        const y = paddingTop + (plotHeight / 4) * i;
        ctx.beginPath();
        ctx.moveTo(paddingLeft, y);
        ctx.lineTo(width - paddingRight, y);
        ctx.stroke();
      }

      // X-Axis Labels
      const timeLabels = ['0', '30m', '1h', '2h', '3h', '4h', '8h', '24h'];
      ctx.fillStyle = '#64748b';
      ctx.font = '10px "JetBrains Mono"';
      ctx.textAlign = 'center';

      timeLabels.forEach((lbl, i) => {
        const x = paddingLeft + (plotWidth / (timeLabels.length - 1)) * i;
        ctx.fillText(lbl, x, height - paddingBottom + 18);
      });

      if (appSamples.length === 0) {
        ctx.fillStyle = '#475569';
        ctx.textAlign = 'center';
        ctx.fillText('No hydrogel groups available to render kinetics', width / 2, height / 2);
        return;
      }

      // Compute max scales for Left (St) and Right (EWC) axes
      let maxStVal = 200;
      let maxEwcVal = 100;

      appSamples.forEach(s => {
        const stats = computeSampleStats(s);
        if (stats.meanEsr + stats.sdEsr > maxStVal) maxStVal = stats.meanEsr + stats.sdEsr;
        if (stats.meanEwc + stats.sdEwc > maxEwcVal) maxEwcVal = stats.meanEwc + stats.sdEwc;

        if (s.timeSeries) {
          Object.values(s.timeSeries).forEach(weights => {
            if (weights && Array.isArray(weights)) {
              const stVals = [];
              const ewcVals = [];
              weights.forEach((wt, rIdx) => {
                const repWd = s.replicates[rIdx] ? s.replicates[rIdx].wd : stats.meanWd;
                if (wt && repWd && wt > repWd) {
                  stVals.push(((wt - repWd) / repWd) * 100);
                  ewcVals.push(((wt - repWd) / wt) * 100);
                }
              });
              const mSt = calcMean(stVals);
              const sdSt = calcSD(stVals);
              const mEwc = calcMean(ewcVals);
              const sdEwc = calcSD(ewcVals);

              if (mSt + sdSt > maxStVal) maxStVal = mSt + sdSt;
              if (mEwc + sdEwc > maxEwcVal) maxEwcVal = mEwc + sdEwc;
            }
          });
        }
      });

      const maxScaleSt = maxStVal * 1.15;
      const maxScaleEwc = Math.min(100, Math.max(10, maxEwcVal * 1.1));

      // Left Y-Axis (S_t Swelling Ratio)
      ctx.textAlign = 'right';
      ctx.fillStyle = '#2dd4bf'; // Teal
      for (let i = 0; i <= 4; i++) {
        const val = Math.round(maxScaleSt * (1 - i / 4));
        const y = paddingTop + (plotHeight / 4) * i + 3;
        ctx.fillText(val + '%', paddingLeft - 8, y);
      }
      ctx.save();
      ctx.translate(14, height / 2);
      ctx.rotate(-Math.PI / 2);
      ctx.textAlign = 'center';
      ctx.font = 'bold 10px "Plus Jakarta Sans"';
      ctx.fillText('S_t Swelling Ratio (%)', 0, 0);
      ctx.restore();

      // Right Y-Axis (EWC Water Content) if Dual
      if (isDual) {
        ctx.textAlign = 'left';
        ctx.fillStyle = '#f472b6'; // Fuchsia
        for (let i = 0; i <= 4; i++) {
          const val = Math.round(maxScaleEwc * (1 - i / 4));
          const y = paddingTop + (plotHeight / 4) * i + 3;
          ctx.fillText(val + '%', width - paddingRight + 8, y);
        }
        ctx.save();
        ctx.translate(width - 12, height / 2);
        ctx.rotate(Math.PI / 2);
        ctx.textAlign = 'center';
        ctx.font = 'bold 10px "Plus Jakarta Sans"';
        ctx.fillText('EWC_t Water Content (%)', 0, 0);
        ctx.restore();
      }

      const colors = ['#10b981', '#f59e0b', '#ec4899', '#8b5cf6', '#06b6d4'];

      appSamples.slice(0, 4).forEach((sample, sIdx) => {
        const stats = computeSampleStats(sample);
        const stPoints = [{ mean: 0, sd: 0 }];
        const ewcPoints = [{ mean: 0, sd: 0 }];

        TIME_INTERVALS.forEach(interval => {
          const weights = sample.timeSeries ? sample.timeSeries[interval.key] : null;
          const stVals = [];
          const ewcVals = [];

          if (weights && Array.isArray(weights)) {
            weights.forEach((wt, rIdx) => {
              const repWd = sample.replicates[rIdx] ? sample.replicates[rIdx].wd : stats.meanWd;
              if (wt && repWd && wt > repWd) {
                stVals.push(((wt - repWd) / repWd) * 100);
                ewcVals.push(((wt - repWd) / wt) * 100);
              }
            });
          }

          if (stVals.length > 0) {
            stPoints.push({ mean: calcMean(stVals), sd: calcSD(stVals) });
            ewcPoints.push({ mean: calcMean(ewcVals), sd: calcSD(ewcVals) });
          } else {
            const k = 0.5;
            stPoints.push({
              mean: stats.meanEsr * (1 - Math.exp(-k * interval.t)),
              sd: stats.sdEsr * (1 - Math.exp(-k * interval.t))
            });
            ewcPoints.push({
              mean: stats.meanEwc * (1 - Math.exp(-k * interval.t)),
              sd: stats.sdEwc * (1 - Math.exp(-k * interval.t))
            });
          }
        });

        const isSelected = sample.id === selectedTimeSeriesSampleId;
        const mainColor = colors[sIdx % colors.length];

        // Draw Swelling Ratio S_t Curve (Solid Line)
        if (chartMetric === 'both' || chartMetric === 'st') {
          ctx.strokeStyle = mainColor;
          ctx.lineWidth = isSelected ? 3 : 2;
          ctx.beginPath();
          stPoints.forEach((pt, i) => {
            const x = paddingLeft + (plotWidth / (timeLabels.length - 1)) * i;
            const y = height - paddingBottom - (pt.mean / (maxScaleSt || 1)) * plotHeight;
            if (i === 0) ctx.moveTo(x, y);
            else ctx.lineTo(x, y);
          });
          ctx.stroke();

          // Error Bars for S_t
          stPoints.forEach((pt, i) => {
            const x = paddingLeft + (plotWidth / (timeLabels.length - 1)) * i;
            const y = height - paddingBottom - (pt.mean / (maxScaleSt || 1)) * plotHeight;
            const yTop = height - paddingBottom - ((pt.mean + pt.sd) / (maxScaleSt || 1)) * plotHeight;
            const yBottom = height - paddingBottom - ((pt.mean - pt.sd) / (maxScaleSt || 1)) * plotHeight;

            if (pt.sd > 0) {
              ctx.strokeStyle = mainColor;
              ctx.lineWidth = 1.2;
              ctx.beginPath();
              ctx.moveTo(x, yTop); ctx.lineTo(x, yBottom);
              ctx.moveTo(x - 3, yTop); ctx.lineTo(x + 3, yTop);
              ctx.moveTo(x - 3, yBottom); ctx.lineTo(x + 3, yBottom);
              ctx.stroke();
            }

            ctx.fillStyle = mainColor;
            ctx.beginPath();
            ctx.arc(x, y, isSelected ? 4.5 : 3, 0, Math.PI * 2);
            ctx.fill();
          });
        }

        // Draw Water Content EWC_t Curve (Dashed Magenta Line)
        if (chartMetric === 'both' || chartMetric === 'ewc') {
          const ewcColor = isDual ? '#f472b6' : mainColor;
          ctx.strokeStyle = ewcColor;
          ctx.lineWidth = isSelected ? 2.5 : 1.5;
          ctx.setLineDash(isDual ? [4, 4] : []);
          ctx.beginPath();
          ewcPoints.forEach((pt, i) => {
            const x = paddingLeft + (plotWidth / (timeLabels.length - 1)) * i;
            const y = height - paddingBottom - (pt.mean / (maxScaleEwc || 1)) * plotHeight;
            if (i === 0) ctx.moveTo(x, y);
            else ctx.lineTo(x, y);
          });
          ctx.stroke();
          ctx.setLineDash([]);

          // Error Bars for EWC
          ewcPoints.forEach((pt, i) => {
            const x = paddingLeft + (plotWidth / (timeLabels.length - 1)) * i;
            const y = height - paddingBottom - (pt.mean / (maxScaleEwc || 1)) * plotHeight;
            const yTop = height - paddingBottom - ((pt.mean + pt.sd) / (maxScaleEwc || 1)) * plotHeight;
            const yBottom = height - paddingBottom - ((pt.mean - pt.sd) / (maxScaleEwc || 1)) * plotHeight;

            if (pt.sd > 0) {
              ctx.strokeStyle = ewcColor;
              ctx.lineWidth = 1.2;
              ctx.beginPath();
              ctx.moveTo(x, yTop); ctx.lineTo(x, yBottom);
              ctx.moveTo(x - 3, yTop); ctx.lineTo(x + 3, yTop);
              ctx.moveTo(x - 3, yBottom); ctx.lineTo(x + 3, yBottom);
              ctx.stroke();
            }

            ctx.fillStyle = ewcColor;
            ctx.beginPath();
            ctx.rect(x - 2.5, y - 2.5, 5, 5);
            ctx.fill();
          });
        }
      });

      // Chart Legend
      ctx.textAlign = 'left';
      ctx.font = '10px "JetBrains Mono"';
      appSamples.slice(0, 3).forEach((sample, sIdx) => {
        const color = colors[sIdx % colors.length];
        const legX = paddingLeft + sIdx * 130;
        ctx.fillStyle = color;
        ctx.beginPath();
        ctx.arc(legX + 4, 16, 4, 0, Math.PI * 2);
        ctx.fill();
        ctx.fillStyle = '#cbd5e1';
        ctx.fillText(sample.id, legX + 12, 19);
      });

      if (isDual) {
        ctx.textAlign = 'right';
        ctx.fillStyle = '#2dd4bf';
        ctx.fillText('— S_t Swelling', width - paddingRight - 100, 19);
        ctx.fillStyle = '#f472b6';
        ctx.fillText('- - EWC_t Water Content', width - paddingRight, 19);
      }
    }

    /**
     * Draw Dynamic Temperature vs Equilibrium Swelling Profile Chart (25°C - 100°C)
     * Plots LCST / UCST curves for ALL samples automatically!
     */
    function drawTemperatureProfileChart() {
      const canvas = document.getElementById('temp-profile-canvas');
      if (!canvas) return;
      const ctx = canvas.getContext('2d');
      if (!ctx) return;

      const rect = canvas.getBoundingClientRect();
      const dpr = window.devicePixelRatio || 1;
      canvas.width = rect.width * dpr;
      canvas.height = rect.height * dpr;
      ctx.scale(dpr, dpr);

      const width = rect.width;
      const height = rect.height;
      ctx.clearRect(0, 0, width, height);

      const paddingLeft = 55;
      const paddingRight = 35;
      const paddingTop = 40;
      const paddingBottom = 40;

      const plotWidth = width - paddingLeft - paddingRight;
      const plotHeight = height - paddingTop - paddingBottom;

      // Draw Grid Lines
      ctx.strokeStyle = '#1e293b';
      ctx.lineWidth = 1;
      for (let i = 0; i <= 4; i++) {
        const y = paddingTop + (plotHeight / 4) * i;
        ctx.beginPath();
        ctx.moveTo(paddingLeft, y);
        ctx.lineTo(width - paddingRight, y);
        ctx.stroke();
      }

      // X-Axis Temperatures (25°C to 100°C)
      const temps = TEMPERATURE_SERIES;
      ctx.fillStyle = '#64748b';
      ctx.font = '9px "JetBrains Mono"';
      ctx.textAlign = 'center';

      temps.forEach((tVal, i) => {
        const x = paddingLeft + (plotWidth / (temps.length - 1)) * i;
        ctx.fillText(tVal + '\u00B0C', x, height - paddingBottom + 16);
      });

      if (appSamples.length === 0) {
        ctx.fillStyle = '#475569';
        ctx.textAlign = 'center';
        ctx.fillText('No hydrogel groups to plot thermo-responsive profile', width / 2, height / 2);
        return;
      }

      // Compute profile data for all samples
      const allSampleProfiles = appSamples.map(sample => computeThermoProfileForSample(sample));

      // Calculate Max Y-Scale based on active mode (seq, ewc, chi)
      let maxYVal = 100;
      let minYVal = 0;

      if (tempChartMode === 'seq') {
        allSampleProfiles.forEach(sp => {
          sp.profile.forEach(p => {
            if (p.Seq + p.sdSeq > maxYVal) maxYVal = p.Seq + p.sdSeq;
          });
        });
        maxYVal = maxYVal * 1.15;
      } else if (tempChartMode === 'ewc') {
        maxYVal = 100;
      } else {
        // Flory Chi parameter chi(T) (typically 0.3 to 0.8)
        minYVal = 0.3;
        maxYVal = 0.8;
      }

      // Draw Y-Axis Labels
      ctx.textAlign = 'right';
      ctx.fillStyle = '#2dd4bf';
      ctx.font = '10px "JetBrains Mono"';

      for (let i = 0; i <= 4; i++) {
        const fraction = 1 - i / 4;
        const val = minYVal + (maxYVal - minYVal) * fraction;
        const y = paddingTop + (plotHeight / 4) * i + 3;
        const labelText = tempChartMode === 'chi' ? val.toFixed(2) : Math.round(val) + '%';
        ctx.fillText(labelText, paddingLeft - 8, y);
      }

      // Y-Axis Title
      ctx.save();
      ctx.translate(14, height / 2);
      ctx.rotate(-Math.PI / 2);
      ctx.textAlign = 'center';
      ctx.font = 'bold 10px "Plus Jakarta Sans"';
      const yTitle = tempChartMode === 'seq' 
        ? 'Equilibrium Swelling Ratio S_eq (%)' 
        : (tempChartMode === 'ewc' ? 'Equilibrium Water Content EWC (%)' : 'Flory-Huggins Interaction Parameter chi(T)');
      ctx.fillText(yTitle, 0, 0);
      ctx.restore();

      const palette = ['#2dd4bf', '#f59e0b', '#ec4899', '#8b5cf6', '#06b6d4'];

      // Draw Phase Transition Lines (Tc)
      allSampleProfiles.forEach((sp) => {
        if (sp.responsiveType !== 'non-responsive' && sp.transitionTemp) {
          const Tc = sp.transitionTemp;
          const minT = temps[0];
          const maxT = temps[temps.length - 1];
          const xTc = paddingLeft + ((Tc - minT) / (maxT - minT)) * plotWidth;

          ctx.strokeStyle = sp.responsiveType === 'lcst' ? 'rgba(45, 212, 191, 0.4)' : 'rgba(245, 158, 11, 0.4)';
          ctx.setLineDash([3, 3]);
          ctx.beginPath();
          ctx.moveTo(xTc, paddingTop);
          ctx.lineTo(xTc, height - paddingBottom);
          ctx.stroke();
          ctx.setLineDash([]);

          ctx.fillStyle = sp.responsiveType === 'lcst' ? '#2dd4bf' : '#f59e0b';
          ctx.font = 'bold 8.5px "Plus Jakarta Sans"';
          ctx.textAlign = 'center';
          ctx.fillText((sp.responsiveType === 'lcst' ? 'LCST ' : 'UCST ') + Tc + '\u00B0C', xTc, paddingTop - 8);
        }
      });

      // Plot Sample Temperature Curves
      allSampleProfiles.forEach((sp, sIdx) => {
        const color = palette[sIdx % palette.length];
        const pts = sp.profile;

        ctx.strokeStyle = color;
        ctx.lineWidth = 2.5;
        ctx.beginPath();

        pts.forEach((pt, i) => {
          const x = paddingLeft + (plotWidth / (temps.length - 1)) * i;
          let yVal = pt.Seq;
          if (tempChartMode === 'ewc') yVal = pt.ewc;
          if (tempChartMode === 'chi') yVal = pt.chi;

          const y = height - paddingBottom - ((yVal - minYVal) / (maxYVal - minYVal)) * plotHeight;
          if (i === 0) ctx.moveTo(x, y);
          else ctx.lineTo(x, y);
        });
        ctx.stroke();

        // Plot Points & Error Bars (for S_eq and EWC)
        pts.forEach((pt, i) => {
          const x = paddingLeft + (plotWidth / (temps.length - 1)) * i;
          let yVal = pt.Seq;
          let sdVal = pt.sdSeq;

          if (tempChartMode === 'ewc') {
            yVal = pt.ewc;
            sdVal = pt.sdEwc;
          }

          const y = height - paddingBottom - ((yVal - minYVal) / (maxYVal - minYVal)) * plotHeight;

          if (tempChartMode !== 'chi' && sdVal > 0) {
            const yTop = height - paddingBottom - (((yVal + sdVal) - minYVal) / (maxYVal - minYVal)) * plotHeight;
            const yBottom = height - paddingBottom - (((yVal - sdVal) - minYVal) / (maxYVal - minYVal)) * plotHeight;

            ctx.strokeStyle = color;
            ctx.lineWidth = 1.2;
            ctx.beginPath();
            ctx.moveTo(x, yTop); ctx.lineTo(x, yBottom);
            ctx.moveTo(x - 2.5, yTop); ctx.lineTo(x + 2.5, yTop);
            ctx.moveTo(x - 2.5, yBottom); ctx.lineTo(x + 2.5, yBottom);
            ctx.stroke();
          }

          ctx.fillStyle = color;
          ctx.beginPath();
          ctx.arc(x, y, 3.5, 0, Math.PI * 2);
          ctx.fill();
        });
      });

      // Legend Header
      ctx.textAlign = 'left';
      ctx.font = '10px "JetBrains Mono"';
      allSampleProfiles.slice(0, 3).forEach((sp, sIdx) => {
        const color = palette[sIdx % palette.length];
        const legX = paddingLeft + sIdx * 140;
        ctx.fillStyle = color;
        ctx.beginPath();
        ctx.arc(legX + 4, 16, 4, 0, Math.PI * 2);
        ctx.fill();
        ctx.fillStyle = '#cbd5e1';
        ctx.fillText(sp.sampleId, legX + 12, 19);
      });
    }

    function openExportModal() {
      const modal = document.getElementById('modal-export');
      const textarea = document.getElementById('export-textarea');
      if (modal) modal.classList.remove('hidden');
      if (!textarea) return;

      let csv = "Group ID,Composition,Thermo Profile,Transition Tc (C),Medium,Replicate #,Dry Mass Wd (mg),Swollen Mass Ws (mg),ESR (%),EWC (%)\n";
      appSamples.forEach(s => {
        if (s.replicates) {
          s.replicates.forEach((r, idx) => {
            const esr = ((r.ws - r.wd) / r.wd * 100).toFixed(2);
            const ewc = ((r.ws - r.wd) / r.ws * 100).toFixed(2);
            csv += '"' + s.id + '","' + s.composition + '","' + (s.responsiveType || 'non-responsive') + '",' + (s.transitionTemp || 'N/A') + ',"' + s.medium + '",' + (idx + 1) + ',' + r.wd + ',' + r.ws + ',' + esr + ',' + ewc + '\n';
          });
        }
      });

      csv += "\nThermo-Responsive Temperature Swelling Matrix (25 C - 100 C):\nGroup ID,Thermo Type,Tc (C)";
      TEMPERATURE_SERIES.forEach(t => {
        csv += ',S_eq at ' + t + 'C (%)';
      });
      csv += '\n';

      appSamples.forEach(s => {
        const prof = computeThermoProfileForSample(s);
        csv += '"' + s.id + '","' + (s.responsiveType || 'non-responsive') + '",' + (s.transitionTemp || 'N/A');
        prof.profile.forEach(p => {
          csv += ',' + p.Seq.toFixed(2);
        });
        csv += '\n';
      });

      csv += "\nProtocol Tasks Status:\nTask Title,Category,Completed\n";
      appTasks.forEach(t => {
        csv += '"' + t.title + '","' + t.category + '",' + (t.completed ? "Yes" : "No") + '\n';
      });

      textarea.value = csv;
    }

    function closeExportModal() {
      const modal = document.getElementById('modal-export');
      if (modal) modal.classList.add('hidden');
    }

    function copyExportToClipboard() {
      const txt = document.getElementById('export-textarea');
      if (!txt) return;
      txt.select();
      document.execCommand('copy');
      showToast('Statistical CSV copied to clipboard!');
    }

    function showToast(msg) {
      const toast = document.getElementById('toast');
      const toastText = document.getElementById('toast-text');
      if (!toast || !toastText) return;

      toastText.innerText = msg;
      toast.classList.remove('translate-y-20', 'opacity-0');

      setTimeout(() => {
        toast.classList.add('translate-y-20', 'opacity-0');
      }, 3000);
    }

    /* EXPLICIT BINDING TO WINDOW OBJECT */
    window.renderTasks = renderTasks;
    window.toggleTask = toggleTask;
    window.filterTasks = filterTasks;
    window.openAddTaskModal = openAddTaskModal;
    window.closeAddTaskModal = closeAddTaskModal;
    window.saveNewTask = saveNewTask;
    window.clearCompletedTasks = clearCompletedTasks;
    window.loadStandardProtocol = loadStandardProtocol;
    window.handleResponsiveTypeChange = handleResponsiveTypeChange;
    window.renderReplicateInputs = renderReplicateInputs;
    window.liveCalculateESR = liveCalculateESR;
    window.handleSampleSubmit = handleSampleSubmit;
    window.clearSampleInputs = clearSampleInputs;
    window.renderSamples = renderSamples;
    window.deleteSample = deleteSample;
    window.populateTimeSeriesSelect = populateTimeSeriesSelect;
    window.switchTimeSeriesSample = switchTimeSeriesSample;
    window.setChartMetric = setChartMetric;
    window.setTempChartMode = setTempChartMode;
    window.renderTimeSeriesGrid = renderTimeSeriesGrid;
    window.updateTimePointReplicateWeight = updateTimePointReplicateWeight;
    window.autoApply24hAsWs = autoApply24hAsWs;
    window.drawKineticsChart = drawKineticsChart;
    window.drawTemperatureProfileChart = drawTemperatureProfileChart;
    window.openExportModal = openExportModal;
    window.closeExportModal = closeExportModal;
    window.copyExportToClipboard = copyExportToClipboard;
    window.showToast = showToast;

    window.addEventListener('DOMContentLoaded', () => {
      refreshIcons();
      renderTasks();
      renderReplicateInputs();
      renderSamples();
      populateTimeSeriesSelect();
      renderTimeSeriesGrid();
      drawKineticsChart();
      drawTemperatureProfileChart();

      window.addEventListener('resize', () => {
        drawKineticsChart();
        drawTemperatureProfileChart();
      });

      // Event delegation for Tasks
      const tasksContainer = document.getElementById('tasks-list');
      if (tasksContainer) {
        tasksContainer.addEventListener('click', (e) => {
          const item = e.target.closest('[data-task-id]');
          if (item) {
            const taskId = item.getAttribute('data-task-id');
            if (taskId) toggleTask(taskId);
          }
        });
      }

      // Event delegation for Sample Deletions
      const tableBody = document.getElementById('samples-table-body');
      if (tableBody) {
        tableBody.addEventListener('click', (e) => {
          const btn = e.target.closest('[data-delete-sample-id]');
          if (btn) {
            const sampleId = btn.getAttribute('data-delete-sample-id');
            if (sampleId) deleteSample(sampleId);
          }
        });
      }

      // Event delegation for Time-Point Replicate Inputs
      const gridContainer = document.getElementById('time-points-grid');
      if (gridContainer) {
        gridContainer.addEventListener('input', (e) => {
          if (e.target && e.target.matches('input[data-time-key]')) {
            const sampleId = e.target.getAttribute('data-sample-id');
            const timeKey = e.target.getAttribute('data-time-key');
            const repIdx = parseInt(e.target.getAttribute('data-rep-idx')) || 0;
            const val = e.target.value;
            if (sampleId && timeKey) {
              updateTimePointReplicateWeight(sampleId, timeKey, repIdx, val);
            }
          }
        });
      }
    });
  </script>
</body>
</html>
