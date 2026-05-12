<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>EO Group Dashboard | Equipment Status</title>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link href="https://fonts.googleapis.com/css2?family=DM+Sans:wght@300;400;500;600;700&family=DM+Mono:wght@400;500&display=swap" rel="stylesheet">
    <style>
        :root {
            --bg: #f4f5f7;
            --surface: #ffffff;
            --surface-2: #f8f9fb;
            --border: #e2e5ea;
            --border-strong: #c8cdd6;
            --text-primary: #0f1923;
            --text-secondary: #5a6475;
            --text-muted: #94a0b3;
            --accent: #1a56db;
            --accent-light: #e8effc;
            --good: #0e9f6e;
            --good-bg: #e8f8f2;
            --warn: #d97706;
            --warn-bg: #fef3e2;
            --crit: #e02424;
            --crit-bg: #fde8e8;
            --shadow-sm: 0 1px 3px rgba(15,25,35,0.07), 0 1px 2px rgba(15,25,35,0.04);
            --shadow-md: 0 4px 12px rgba(15,25,35,0.09), 0 2px 4px rgba(15,25,35,0.05);
            --shadow-lg: 0 10px 30px rgba(15,25,35,0.12), 0 4px 10px rgba(15,25,35,0.07);
            --radius: 10px;
            --radius-sm: 6px;
            --topbar-tab-width: 128px;
        }

        * { box-sizing: border-box; margin: 0; padding: 0; }

        html { scrollbar-gutter: stable; }

        body {
            font-family: 'DM Sans', sans-serif;
            background: var(--bg);
            color: var(--text-primary);
            font-size: 14px;
            line-height: 1.5;
            overflow-y: scroll;
        }

        /* ─── HEADER ─── */
        header {
            background: var(--surface);
            border-bottom: 1px solid var(--border);
            padding: 0 28px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            height: 62px;
            position: sticky;
            top: 0;
            z-index: 100;
            box-shadow: var(--shadow-sm);
        }

        .header-left { display: flex; align-items: center; gap: 14px; }

        .logo-badge {
            background: var(--accent);
            color: white;
            font-weight: 700;
            font-size: 11px;
            letter-spacing: 1.5px;
            padding: 5px 9px;
            border-radius: 5px;
            font-family: 'DM Mono', monospace;
        }

        header h1 {
            font-size: 15px;
            font-weight: 600;
            color: var(--text-primary);
            letter-spacing: -0.2px;
        }

        header h1 span {
            color: var(--text-muted);
            font-weight: 400;
        }

        .header-right { display: flex; align-items: center; gap: 18px; }

        .timestamp {
            font-size: 12px;
            color: var(--text-muted);
            font-family: 'DM Mono', monospace;
            background: var(--bg);
            padding: 5px 10px;
            border-radius: 5px;
            border: 1px solid var(--border);
        }

        .live-dot {
            width: 7px; height: 7px;
            background: var(--good);
            border-radius: 50%;
            display: inline-block;
            margin-right: 5px;
            animation: pulse-dot 2s infinite;
        }

        @keyframes pulse-dot {
            0%, 100% { opacity: 1; }
            50% { opacity: 0.4; }
        }

        .login-btn {
            background: var(--accent);
            color: white;
            border: none;
            padding: 8px 16px;
            border-radius: var(--radius-sm);
            font-size: 13px;
            font-weight: 600;
            cursor: pointer;
            display: flex;
            align-items: center;
            gap: 7px;
            font-family: 'DM Sans', sans-serif;
            transition: background 0.2s, transform 0.1s;
            letter-spacing: 0.1px;
        }
        .login-btn:hover { background: #1447c0; transform: translateY(-1px); }
        .login-btn:active { transform: translateY(0); }

        /* ─── MAIN LAYOUT ─── */
        main { padding: 24px 28px; max-width: 1400px; margin: 0 auto; }

        /* ─── SECTION HEADERS ─── */
        .section-label {
            font-size: 11px;
            font-weight: 600;
            letter-spacing: 1.2px;
            text-transform: uppercase;
            color: var(--text-muted);
            margin-bottom: 12px;
            display: flex;
            align-items: center;
            gap: 8px;
        }
        .section-label::after {
            content: '';
            flex: 1;
            height: 1px;
            background: var(--border);
        }

        /* ─── KPI ROW ─── */
        .kpi-row {
            display: grid;
            grid-template-columns: 180px 160px 1fr;
            gap: 16px;
            margin-bottom: 24px;
        }

        .kpi-card {
            background: var(--surface);
            border: 1px solid var(--border);
            border-radius: var(--radius);
            padding: 20px;
            box-shadow: var(--shadow-sm);
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            gap: 10px;
        }

        .kpi-label {
            font-size: 11px;
            font-weight: 600;
            letter-spacing: 0.8px;
            text-transform: uppercase;
            color: var(--text-muted);
        }

        /* Health gauge */
        .health-ring {
            width: 90px; height: 90px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 17px;
            font-weight: 700;
            font-family: 'DM Mono', monospace;
            transition: background 0.8s;
            position: relative;
        }
        .health-ring::after {
            content: '';
            position: absolute;
            inset: 8px;
            border-radius: 50%;
            background: white;
        }
        .health-ring span { position: relative; z-index: 1; font-size: 14px; }

        /* Breakdown count */
        .kpi-num {
            font-size: 44px;
            font-weight: 700;
            font-family: 'DM Mono', monospace;
            line-height: 1;
        }

        /* Summary table */
        .kpi-table-card { align-items: stretch; padding: 16px 20px; }
        .kpi-table-card .kpi-label { align-self: flex-start; }

        .summary-table { width: 100%; border-collapse: collapse; }
        .summary-table th {
            text-align: center;
            padding: 5px 8px;
            font-size: 11px;
            font-weight: 600;
            text-transform: uppercase;
            letter-spacing: 0.6px;
            color: var(--text-muted);
            border-bottom: 1px solid var(--border);
        }
        .summary-table td {
            text-align: center;
            padding: 8px 8px;
            font-weight: 600;
            font-size: 13px;
            font-family: 'DM Mono', monospace;
        }
        .summary-table td:first-child {
            text-align: left;
            color: var(--text-secondary);
            font-weight: 500;
            font-family: 'DM Sans', sans-serif;
            font-size: 12px;
        }
        .summary-table tr + tr td { border-top: 1px solid var(--bg); }

        .gauge-row { display: grid; grid-template-columns: repeat(3,1fr); gap:16px; margin-bottom:24px; } /* legacy */

        .gauge-card {
            background: var(--surface);
            border: 1px solid var(--border);
            border-radius: var(--radius);
            box-shadow: var(--shadow-sm);
            position: relative;
            overflow: hidden;
        }
        .gauge-card::before {
            content: '';
            position: absolute;
            top: 0; left: 0; right: 0;
            height: 3px;
            background: var(--border);
            transition: background 0.3s;
        }
        .gauge-card:hover { border-color: var(--accent); box-shadow: var(--shadow-md); transform: translateY(-2px); }
        .gauge-card:hover::before { background: var(--accent); }
        .gauge-card.active { border-color: var(--accent); box-shadow: 0 0 0 3px var(--accent-light), var(--shadow-md); }
        .gauge-card.active::before { background: var(--accent); }

        .gauge-dept-name {
            font-size: 12px;
            font-weight: 600;
            letter-spacing: 0.8px;
            text-transform: uppercase;
            color: var(--text-secondary);
            display: flex; align-items: center; gap: 6px;
        }

        /* Semi-circle gauge */
        .semi-gauge-wrap {
            position: relative;
            width: 130px;
            height: 68px;
            overflow: hidden;
        }
        .semi-gauge-wrap::before {
            content: '';
            position: absolute;
            top: 0; left: 0;
            width: 130px; height: 130px;
            border-radius: 50%;
            border: 14px solid #e8ecf8;
            box-sizing: border-box;
        }
        .gauge-fill {
            position: absolute;
            top: 0; left: 0;
            width: 130px; height: 130px;
            border-radius: 50%;
            border: 14px solid;
            box-sizing: border-box;
            border-bottom-color: transparent !important;
            border-left-color: transparent !important;
            transform: rotate(-45deg);
            transition: transform 1s ease, border-color 0.5s;
        }
        .gauge-pct {
            position: absolute;
            bottom: -4px;
            width: 100%;
            text-align: center;
            font-size: 22px;
            font-weight: 700;
            font-family: 'DM Mono', monospace;
            color: var(--text-primary);
        }
        .gauge-hint {
            font-size: 11px;
            color: var(--accent);
            font-weight: 600;
            letter-spacing: 0.5px;
        }

        /* ─── DISCIPLINE / ASSET COLOUR TOKENS ─── */
        :root {
            --disc-bg:   #eef2ff;   /* indigo tint for discipline panel */
            --disc-bdr:  #c7d2fe;
            --asset-bg:  #f0fdf4;   /* green tint for asset panel */
            --asset-bdr: #bbf7d0;

            --elec-col:  #2563eb;   /* blue */
            --elec-light:#dbeafe;
            --mech-col:  #d97706;   /* amber */
            --mech-light:#fef3c7;
            --inst-col:  #059669;   /* green */
            --inst-light:#d1fae5;
            --bm-col:    #92400e;   /* brown */
            --bm-light:  #fef3c7;
        }

        /* ─── DUAL PANEL LAYOUT ─── */
        .dual-panel {
            display: grid;
            grid-template-columns: 300px 1fr;
            gap: 0;
            margin-bottom: 24px;
            align-items: stretch;
            border-radius: var(--radius);
            overflow: hidden;
            box-shadow: var(--shadow-md);
            border: 1px solid var(--border);
        }

        .dual-left {
            display: flex;
            flex-direction: column;
            background: var(--disc-bg);
            border-right: 1px solid var(--disc-bdr);
            padding: 18px 16px;
        }

        .dual-right {
            display: flex;
            flex-direction: column;
            background: var(--asset-bg);
            padding: 18px 16px;
        }

        /* Section labels inside panels */
        .panel-section-label {
            font-size: 11px;
            font-weight: 700;
            letter-spacing: 1.2px;
            text-transform: uppercase;
            margin-bottom: 14px;
            display: flex;
            align-items: center;
            gap: 7px;
        }
        .panel-section-label.disc { color: #4338ca; }
        .panel-section-label.asset { color: #065f46; }

        /* Stacked discipline gauges */
        .gauge-stack { display: flex; flex-direction: column; gap: 10px; }

        .gauge-card-wide {
            padding: 14px 16px;
            cursor: pointer;
            transition: border-color 0.2s, box-shadow 0.2s, transform 0.15s;
            border-radius: 8px;
            border: 1.5px solid transparent;
            background: white;
            box-shadow: var(--shadow-sm);
        }
        .gauge-card-wide:hover { transform: translateX(3px); box-shadow: var(--shadow-md); }

        /* Per-discipline colours */
        #card-elec { border-color: #bfdbfe; }
        #card-elec:hover { border-color: var(--elec-col); background: #f0f7ff; box-shadow: 0 0 0 2px var(--elec-light), var(--shadow-md); }
        #card-elec.active { border-color: var(--elec-col); background: #1d4ed8; box-shadow: 0 0 0 3px var(--elec-light), var(--shadow-md); }
        #card-elec.active .gauge-dept-name { color: white; }
        #card-elec.active .gauge-hint { color: #bfdbfe; opacity:1; }
        #card-elec.active .gauge-pct { color: white !important; }
        #card-elec::before { background: var(--elec-col); }

        #card-mech { border-color: #fde68a; }
        #card-mech:hover { border-color: var(--mech-col); background: #fffbeb; box-shadow: 0 0 0 2px var(--mech-light), var(--shadow-md); }
        #card-mech.active { border-color: var(--mech-col); background: #b45309; box-shadow: 0 0 0 3px var(--mech-light), var(--shadow-md); }
        #card-mech.active .gauge-dept-name { color: white; }
        #card-mech.active .gauge-hint { color: #fde68a; opacity:1; }
        #card-mech.active .gauge-pct { color: white !important; }
        #card-mech::before { background: var(--mech-col); }

        #card-inst { border-color: #a7f3d0; }
        #card-inst:hover { border-color: var(--inst-col); background: #f0fdf8; box-shadow: 0 0 0 2px var(--inst-light), var(--shadow-md); }
        #card-inst.active { border-color: var(--inst-col); background: #065f46; box-shadow: 0 0 0 3px var(--inst-light), var(--shadow-md); }
        #card-inst.active .gauge-dept-name { color: white; }
        #card-inst.active .gauge-hint { color: #a7f3d0; opacity:1; }
        #card-inst.active .gauge-pct { color: white !important; }
        #card-inst::before { background: var(--inst-col); }

        #card-bm { border-color: #d6b46a; }
        #card-bm:hover { border-color: var(--bm-col); background: #fffbeb; box-shadow: 0 0 0 2px var(--bm-light), var(--shadow-md); }
        #card-bm.active { border-color: var(--bm-col); background: #92400e; box-shadow: 0 0 0 3px var(--bm-light), var(--shadow-md); }
        #card-bm.active .gauge-dept-name { color: white; }
        #card-bm.active .gauge-hint { color: #fde68a; opacity:1; }
        #card-bm.active .gauge-pct { color: white !important; }
        #card-bm::before { background: var(--bm-col); }

        /* Dept name colours when NOT active */
        #card-elec:not(.active) .gauge-dept-name { color: var(--elec-col); }
        #card-mech:not(.active) .gauge-dept-name { color: var(--mech-col); }
        #card-inst:not(.active) .gauge-dept-name { color: var(--inst-col); }
        #card-bm:not(.active)   .gauge-dept-name { color: var(--bm-col); }
        #card-elec:not(.active) .gauge-hint { color: var(--elec-col); opacity:0.7; }
        #card-mech:not(.active) .gauge-hint { color: var(--mech-col); opacity:0.7; }
        #card-inst:not(.active) .gauge-hint { color: var(--inst-col); opacity:0.7; }
        #card-bm:not(.active)   .gauge-hint { color: var(--bm-col);   opacity:0.7; }

        /* Semi-gauge track stays visible on dark active bg */
        .gauge-card-wide.active .semi-gauge-wrap::before { border-color: rgba(255,255,255,0.2); }

        .gauge-row-inner {
            display: flex;
            align-items: center;
            justify-content: space-between;
            gap: 12px;
        }

        .gauge-left-meta {
            display: flex;
            flex-direction: column;
            gap: 4px;
        }

        /* ─── SITE GRID ─── */
        .site-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(110px, 1fr));
            gap: 10px;
            flex: 1;
        }

        /* ─── SITE SECTION ─── (legacy) */
        .site-scroll {
            display: flex;
            gap: 12px;
            overflow-x: auto;
            padding-bottom: 6px;
            scrollbar-width: thin;
            scrollbar-color: var(--border) transparent;
        }

        .site-card {
            background: white;
            border: 1.5px solid var(--asset-bdr);
            border-radius: var(--radius);
            padding: 12px 8px;
            text-align: center;
            cursor: pointer;
            transition: all 0.18s;
            box-shadow: var(--shadow-sm);
            min-height: 110px;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
        }
        .site-card:hover { border-color: #34d399; transform: translateY(-2px); box-shadow: var(--shadow-md); }
        .site-card.active {
            border-color: #065f46;
            background: #065f46;
            box-shadow: 0 0 0 3px rgba(5,150,105,0.25), var(--shadow-md);
            transform: translateY(-2px);
        }
        .site-card.active .site-name { color: white; }
        .site-card.active .circle-bg { stroke: rgba(255,255,255,0.15); }
        .site-card.active svg text { fill: white !important; }
        .site-card.active svg line { stroke: rgba(255,255,255,0.3) !important; }

        /* Ghost placeholder cards */
        .site-card-ghost {
            background: white;
            border: 1.5px solid #d1f0e0;
            border-radius: var(--radius);
            min-height: 110px;
            box-shadow: var(--shadow-sm);
        }

        .site-icon { font-size: 22px; margin-bottom: 5px; }
        .site-name { font-size: 12px; font-weight: 700; letter-spacing: 0.8px; color: var(--text-primary); margin-bottom: 3px; }

        .circular-chart { display: block; margin: 8px auto 0; width: 60px; height: 60px; }
        .circle-bg { fill: none; stroke: var(--bg); stroke-width: 3; }
        .circle { fill: none; stroke-width: 4; stroke-linecap: round; transition: stroke-dasharray 1s ease-out; }

        /* ─── DETAILS PANEL ─── */
        .details-panel {
            display: none;
            background: var(--surface);
            border: 1px solid var(--border);
            border-radius: var(--radius);
            padding: 24px;
            margin-top: 20px;
            box-shadow: var(--shadow-md);
            animation: slideDown 0.25s ease-out;
        }
        @keyframes slideDown { from { opacity: 0; transform: translateY(-8px); } to { opacity: 1; transform: translateY(0); } }

        .panel-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            border-bottom: 1px solid var(--border);
            padding-bottom: 14px;
            margin-bottom: 20px;
        }
        .panel-header h2 { font-size: 15px; font-weight: 600; }
        .close-btn {
            width: 28px; height: 28px;
            border: 1px solid var(--border);
            background: var(--bg);
            border-radius: 50%;
            display: flex; align-items: center; justify-content: center;
            cursor: pointer;
            font-size: 13px;
            color: var(--text-muted);
            transition: 0.15s;
        }
        .close-btn:hover { background: var(--crit-bg); color: var(--crit); border-color: var(--crit); }

        .detail-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 24px; }

        .chart-title {
            font-size: 11px;
            font-weight: 600;
            letter-spacing: 1px;
            text-transform: uppercase;
            color: var(--text-muted);
            margin-bottom: 14px;
        }

        /* Bar chart */
        .bar-row { display: flex; align-items: center; gap: 10px; margin-bottom: 10px; }
        .bar-label { width: 130px; font-size: 12px; color: var(--text-secondary); }
        .bar-track { flex-grow: 1; height: 8px; background: var(--bg); border-radius: 4px; overflow: hidden; }
        .bar-fill { height: 100%; border-radius: 4px; }
        .bar-value { width: 38px; text-align: right; font-size: 12px; font-weight: 600; font-family: 'DM Mono', monospace; color: var(--text-secondary); }

        /* Site filter */
        .site-filter-row { display: grid; grid-template-columns: repeat(3, 1fr); gap: 12px; margin-bottom: 20px; }
        .dept-stat-box {
            background: var(--surface-2);
            border: 1.5px solid var(--border);
            border-radius: var(--radius-sm);
            padding: 14px;
            text-align: center;
            cursor: pointer;
            transition: all 0.2s;
        }
        .dept-stat-box:hover { border-color: var(--accent); }
        .dept-stat-box.active { border-color: var(--accent); background: var(--accent-light); }
        .dept-stat-box h4 { font-size: 11px; font-weight: 600; letter-spacing: 0.6px; text-transform: uppercase; color: var(--text-muted); margin-bottom: 8px; pointer-events: none; }
        .dept-stat-box .numbers { font-size: 20px; font-family: 'DM Mono', monospace; pointer-events: none; font-weight: 700; }
        .dept-stat-box .click-hint { font-size: 10px; color: var(--accent); margin-top: 6px; pointer-events: none; font-weight: 600; letter-spacing: 0.3px; }

        /* ─── ISSUE CARDS ─── */
        .issue-list { display: flex; flex-direction: column; gap: 10px; }
        .issue-card {
            border: 1px solid var(--border);
            border-left: 4px solid;
            border-radius: var(--radius-sm);
            padding: 14px 16px;
            cursor: pointer;
            transition: box-shadow 0.15s, transform 0.1s;
            background: var(--surface);
        }
        .issue-card:hover { box-shadow: var(--shadow-md); transform: translateX(2px); }
        .issue-header { display: flex; justify-content: space-between; align-items: flex-start; }
        .issue-info h3 { font-size: 13px; font-weight: 600; margin-bottom: 3px; }
        .issue-info p { font-size: 11px; color: var(--text-muted); }
        .issue-badge {
            padding: 3px 9px;
            border-radius: 20px;
            font-size: 11px;
            font-weight: 600;
            letter-spacing: 0.3px;
            display: inline-flex;
            align-items: center;
            gap: 4px;
            white-space: nowrap;
        }
        .badge-crit { background: var(--crit-bg); color: var(--crit); }
        .badge-warn { background: var(--warn-bg); color: var(--warn); }
        .card-footer-hint { font-size: 10px; color: var(--accent); margin-top: 6px; text-align: right; font-weight: 600; }

        /* ─── BACKEND ─── */
        #backend-view { display: none; }

        .backend-header {
            background: var(--surface);
            border: 1px solid var(--border);
            border-radius: var(--radius);
            padding: 16px 20px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
            box-shadow: var(--shadow-sm);
        }
        .backend-header h2 { font-size: 16px; font-weight: 700; }
        .role-badge {
            background: var(--accent-light);
            color: var(--accent);
            padding: 2px 10px;
            border-radius: 20px;
            font-size: 11px;
            font-weight: 700;
            letter-spacing: 0.5px;
        }

        .backend-grid { display: grid; grid-template-columns: 1fr 2fr; gap: 16px; }
        .entry-form-card, .data-table-card {
            background: var(--surface);
            padding: 20px;
            border-radius: var(--radius);
            border: 1px solid var(--border);
            box-shadow: var(--shadow-sm);
        }
        .entry-form-card h3, .data-table-card h3 {
            font-size: 13px;
            font-weight: 700;
            color: var(--accent);
            margin-bottom: 16px;
            text-transform: uppercase;
            letter-spacing: 0.8px;
        }

        .form-group { margin-bottom: 12px; }
        .form-group label {
            display: block;
            font-size: 11px;
            font-weight: 600;
            color: var(--text-muted);
            text-transform: uppercase;
            letter-spacing: 0.6px;
            margin-bottom: 5px;
        }
        .form-group input,
        .form-group select,
        .form-group textarea {
            width: 100%;
            padding: 8px 11px;
            background: var(--bg);
            border: 1px solid var(--border);
            color: var(--text-primary);
            border-radius: var(--radius-sm);
            font-family: 'DM Sans', sans-serif;
            font-size: 13px;
            transition: border-color 0.15s;
        }
        .form-group input:focus,
        .form-group select:focus,
        .form-group textarea:focus {
            outline: none;
            border-color: var(--accent);
            box-shadow: 0 0 0 3px var(--accent-light);
        }
        .form-group input:disabled {
            background: var(--bg);
            color: var(--text-muted);
        }

        .btn-primary {
            background: var(--accent);
            color: white;
            border: none;
            padding: 9px 16px;
            width: 100%;
            border-radius: var(--radius-sm);
            font-weight: 600;
            cursor: pointer;
            margin-top: 8px;
            font-family: 'DM Sans', sans-serif;
            font-size: 13px;
            transition: background 0.15s;
        }
        .btn-primary:hover { background: #1447c0; }

        .btn-danger {
            background: var(--crit-bg);
            color: var(--crit);
            border: 1px solid rgba(224,36,36,0.2);
            padding: 4px 10px;
            border-radius: var(--radius-sm);
            cursor: pointer;
            font-size: 11px;
            font-weight: 600;
            font-family: 'DM Sans', sans-serif;
        }
        .btn-danger:hover { background: var(--crit); color: white; }

        .data-table { width: 100%; border-collapse: collapse; font-size: 12px; }
        .data-table th {
            padding: 9px 12px;
            text-align: left;
            font-size: 10px;
            font-weight: 600;
            text-transform: uppercase;
            letter-spacing: 0.8px;
            color: var(--text-muted);
            border-bottom: 1px solid var(--border);
            background: var(--bg);
        }
        .data-table td {
            padding: 9px 12px;
            border-bottom: 1px solid var(--border);
            color: var(--text-primary);
        }
        .data-table tr:last-child td { border-bottom: none; }
        .data-table tr:hover td { background: var(--bg); }

        /* ─── AUTH OVERLAY ─── */
        .auth-overlay {
            position: fixed; inset: 0;
            background: rgba(15,25,35,0.55);
            backdrop-filter: blur(4px);
            display: none;
            justify-content: center;
            align-items: center;
            z-index: 2000;
        }
        .auth-box {
            background: var(--surface);
            padding: 32px 28px;
            border-radius: 14px;
            border: 1px solid var(--border);
            width: 340px;
            box-shadow: var(--shadow-lg);
            animation: slideDown 0.2s ease-out;
        }
        .auth-box h2 {
            font-size: 18px;
            font-weight: 700;
            margin-bottom: 4px;
        }
        .auth-box p {
            font-size: 12px;
            color: var(--text-muted);
            margin-bottom: 20px;
        }
        .auth-error {
            background: var(--crit-bg);
            color: var(--crit);
            border-radius: var(--radius-sm);
            padding: 8px 12px;
            font-size: 12px;
            font-weight: 600;
            margin-bottom: 10px;
            display: none;
        }
        /* ─── PORTAL TABS ─── */
        .portal-tabs {
            display: flex;
            gap: 4px;
            margin-bottom: 20px;
            border-bottom: 2px solid var(--border);
            padding-bottom: 0;
        }
        .ptab {
            background: none;
            border: none;
            padding: 9px 16px;
            font-size: 13px;
            font-weight: 600;
            color: var(--text-muted);
            cursor: pointer;
            border-bottom: 2px solid transparent;
            margin-bottom: -2px;
            font-family: 'DM Sans', sans-serif;
            border-radius: 6px 6px 0 0;
            transition: color 0.15s, border-color 0.15s;
        }
        .ptab:hover { color: var(--text-primary); background: var(--bg); }
        .ptab.active { color: var(--accent); border-bottom-color: var(--accent); background: var(--surface); }

        /* ─── BTN SECONDARY ─── */
        .btn-secondary {
            background: #e9edf3;
            color: #425066;
            border: 1px solid #c5ced9;
            padding: 9px 16px;
            width: 100%;
            border-radius: var(--radius-sm);
            font-weight: 600;
            cursor: pointer;
            margin-top: 8px;
            font-family: 'DM Sans', sans-serif;
            font-size: 13px;
        }
        .btn-secondary:hover { background: #dde4ec; }
        .btn-secondary.active {
            background: var(--accent);
            color: #fff;
            border-color: var(--accent);
        }

        /* ─── MODAL ─── */
        .modal-overlay {
            position: fixed; inset: 0;
            background: rgba(15,25,35,0.5);
            backdrop-filter: blur(3px);
            display: none;
            justify-content: center;
            align-items: center;
            z-index: 1000;
        }
        .modal-content {
            background: var(--surface);
            border: 1px solid var(--border);
            border-radius: 14px;
            width: 92%;
            max-width: 620px;
            padding: 28px;
            box-shadow: var(--shadow-lg);
            animation: slideDown 0.2s ease-out;
        }
        .modal-header {
            display: flex;
            justify-content: space-between;
            align-items: flex-start;
            border-bottom: 1px solid var(--border);
            padding-bottom: 14px;
            margin-bottom: 20px;
        }
        .modal-header h2 { font-size: 16px; font-weight: 700; }
        .modal-header .sub-text { font-size: 12px; color: var(--text-muted); margin-top: 3px; display: block; font-family: 'DM Mono', monospace; }

        .detail-row { margin-bottom: 14px; }
        .detail-row label {
            display: block;
            font-size: 10px;
            font-weight: 600;
            text-transform: uppercase;
            letter-spacing: 0.8px;
            color: var(--text-muted);
            margin-bottom: 5px;
        }
        .detail-value {
            font-size: 13px;
            background: var(--bg);
            padding: 10px 12px;
            border-radius: var(--radius-sm);
            border: 1px solid var(--border);
            line-height: 1.5;
        }

        /* ─── ADMIN SECTIONS ─── */
        #admin-tools {
            display: none;
            margin-bottom: 20px;
        }

        /* ─── STATUS COLORS ─── */
        .status-good { color: var(--good); }
        .status-ok   { color: var(--good); }
        .status-warn { color: var(--warn); }
        .status-crit { color: var(--crit); }

        /* ─── EMPTY STATE ─── */
        .empty-state {
            padding: 16px;
            border: 1px solid var(--good);
            border-radius: var(--radius-sm);
            background: var(--good-bg);
            color: var(--good);
            font-size: 13px;
            font-weight: 600;
            text-align: center;
        }

        /* ─── PANEL SPLIT LAYOUT ─── */
        .panel-split {
            display: grid;
            grid-template-columns: 240px 1fr;
            gap: 24px;
            align-items: start;
        }
        .panel-split-left { }
        .panel-split-right { }

        /* ─── ISSUE TABLE ─── */
        .issue-tbl {
            width: 100%;
            border-collapse: collapse;
            font-size: 12px;
        }
        .issue-tbl thead tr {
            background: var(--bg);
        }
        .issue-tbl th {
            padding: 8px 12px;
            text-align: left;
            font-size: 10px;
            font-weight: 700;
            text-transform: uppercase;
            letter-spacing: 0.8px;
            color: var(--text-muted);
            border-bottom: 2px solid var(--border);
            white-space: nowrap;
        }
        .issue-tbl th:nth-child(3),
        .issue-tbl th:nth-child(4),
        .issue-tbl th:nth-child(5),
        .issue-tbl th:nth-child(6),
        .issue-tbl th:nth-child(7) { text-align: center; }

        .issue-table-row {
            border-bottom: 1px solid var(--border);
            transition: background 0.12s;
        }
        .issue-table-row:hover { background: var(--surface-2); }
        .issue-table-row td { vertical-align: middle; }

        /* Days badge */
        .days-badge {
            display: inline-block;
            padding: 3px 9px;
            border-radius: 20px;
            font-size: 11px;
            font-weight: 700;
            font-family: 'DM Mono', monospace;
        }
        .days-badge-status-crit { background: var(--crit-bg);  color: var(--crit); }
        .days-badge-status-warn { background: var(--warn-bg);  color: var(--warn); }
        .days-badge-status-ok   { background: var(--good-bg);  color: var(--good); }

        /* ─── EXPORT DROPDOWN ANIMATION ─── */
        #export-dropdown { animation: fadeIn 0.12s ease-out; }
        @keyframes fadeIn { from { opacity:0; transform:translateY(-4px); } to { opacity:1; transform:translateY(0); } }

        /* ─── RESPONSIVE ─── */
        @media (max-width: 960px) {
            .kpi-row { grid-template-columns: 1fr 1fr; }
            .kpi-row > :last-child { grid-column: 1 / -1; }
            .dual-panel { grid-template-columns: 1fr; }
            .dual-left { border-right: none; border-bottom: 1px solid var(--disc-bdr); }
            .gauge-stack { flex-direction: row; flex-wrap: wrap; }
            .gauge-card-wide { flex: 1; min-width: 180px; }
            .panel-split { grid-template-columns: 1fr; }
            .detail-grid { grid-template-columns: 1fr; }
            .backend-grid { grid-template-columns: 1fr; }
            .site-filter-row { grid-template-columns: 1fr; }
            main { padding: 16px; }
        }
    /* ─── TOP NAV BAR ─── */
    .topbar{background:#0d1f3c;color:#fff;display:flex;align-items:stretch;gap:0;min-height:48px;position:sticky;top:0;z-index:1000;font-family:'Segoe UI',Arial,sans-serif;}
.topbar .brand{font-size:15px;font-weight:800;white-space:nowrap;padding:0 20px;border-right:1px solid rgba(255,255,255,.14);display:flex;align-items:center;flex:0 0 190px;min-width:190px;}
.topbar nav{display:flex;align-items:stretch;flex:0 0 auto;}
.topbar nav a{color:rgba(255,255,255,.78);text-decoration:none;padding:0 12px;font-size:13px;font-weight:700;display:flex;align-items:center;justify-content:center;white-space:nowrap;border-bottom:3px solid transparent;flex:0 0 var(--topbar-tab-width);width:var(--topbar-tab-width);min-height:48px;min-width:0;box-sizing:border-box;}
.topbar nav a:hover{background:transparent;color:rgba(255,255,255,.78);}
.topbar nav a.active{background:rgba(255,255,255,.1);color:#fff;border-bottom:3px solid #4a9eff;}
.topbar .spacer{flex:1;}
.topbar .user-pill{display:flex;align-items:center;gap:10px;padding:0 4px;font-size:13px;color:rgba(255,255,255,.85);}
.topbar .user-pill span{font-weight:500;}
.topbar .user-pill button{background:rgba(255,255,255,.12);border:1px solid rgba(255,255,255,.25);color:#fff;padding:5px 14px;border-radius:4px;cursor:pointer;font-size:12px;font-weight:600;}
.topbar .user-pill button:hover{background:rgba(255,255,255,.22);}
.topbar-login-btn{background:rgba(255,255,255,.12);border:1px solid rgba(255,255,255,.25);color:#fff;padding:5px 14px;border-radius:4px;cursor:pointer;font-size:12px;font-weight:600;margin-right:16px;transition:background .15s;white-space:nowrap;}
.topbar-login-btn:hover{background:rgba(255,255,255,.22);}
@media (max-width:1100px){
    .topbar{flex-wrap:wrap;}
    .topbar .brand{flex-basis:170px;min-width:170px;}
    .topbar nav{order:3;width:100%;border-top:1px solid rgba(255,255,255,.1);}
    .topbar nav a{flex-basis:auto;width:auto;min-width:0;padding:0 12px;}
    .topbar .spacer{display:none;}
    .topbar .user-pill,.topbar-login-btn{margin:6px 12px 6px 0;}
}
    .topbar .tb-time{font-size:11px;color:rgba(255,255,255,.5);font-family:'DM Mono',monospace;padding:0 10px 0 0;white-space:nowrap;}
    .topbar .tb-btn{background:rgba(255,255,255,.1);border:1px solid rgba(255,255,255,.2);color:#fff;padding:5px 12px;border-radius:4px;cursor:pointer;font-size:12px;font-weight:600;transition:background .15s;white-space:nowrap;margin-right:6px;}
    .topbar .tb-btn:hover{background:rgba(255,255,255,.2);}
    /* Hide the old white header bar — topbar replaces it */
    header { display: none !important; }
    /* ─── LOGIN WALL ─── */
    #login-wall{position:fixed;inset:0;display:flex;align-items:center;justify-content:center;padding:20px;background:#f3f6fb;z-index:999;overflow-y:auto;}
    .lw-card{background:#fff;border-radius:12px;padding:40px 36px;width:90%;max-width:500px;flex-shrink:0;border:1px solid #d8dee6;border-top:4px solid #1a56db;box-shadow:0 4px 24px rgba(13,31,60,.12);text-align:center;}
    .lw-icon{font-size:56px;margin-bottom:16px;display:block;}
    .lw-card h2{font-size:20px;font-weight:800;color:#0d1f3c;margin:0 0 8px;}
    .lw-card>p{font-size:13px;color:#6b7a90;line-height:1.7;margin:0 0 20px;}
    .lw-access{width:100%;border-collapse:collapse;margin:0 0 20px;text-align:left;}
    .lw-access tr{border-bottom:1px solid #e8edf3;}
    .lw-access tr:last-child{border-bottom:none;}
    .lw-access td{padding:8px 10px;font-size:13px;vertical-align:top;}
    .lw-access td:first-child{font-weight:700;color:#0d1f3c;width:44%;white-space:nowrap;}
    .lw-access td:last-child{color:#6b7a90;}
    .lw-btn{display:inline-block;background:#1a56db;color:#fff;border:none;border-radius:8px;padding:12px 40px;font-size:14px;font-weight:700;cursor:pointer;font-family:inherit;transition:background .15s;}
    .lw-btn:hover{background:#1447c0;}
    /* ─── PERMISSION MATRIX ─── */
    .perm-group{border-bottom:1px solid var(--border);}
    .perm-group:last-child{border-bottom:none;}
    .perm-group-hdr{padding:6px 10px;font-size:11px;font-weight:700;background:var(--bg);color:var(--text-secondary);letter-spacing:0.5px;border-bottom:1px solid var(--border);}
    .perm-items{display:grid;grid-template-columns:1fr 1fr;}
    .perm-item{display:flex;align-items:center;gap:7px;padding:5px 10px;font-size:12px;font-weight:400;color:var(--text-primary);cursor:pointer;border-bottom:1px solid var(--border);margin:0;}
    .perm-item:hover{background:var(--accent-light);}
    .perm-item input[type=checkbox]{cursor:pointer;width:13px;height:13px;flex-shrink:0;}
    </style>

<style>
/* ── WORKFLOW PRE-LOGIN SCREEN ─────────────────────────────────────────────── */
#login-wall{background:#f0f4fa!important;}
.wf-wrap{max-width:1200px;margin:0 auto;padding:36px 32px 56px;width:100%;}
.wf-top{display:flex;align-items:flex-start;justify-content:space-between;gap:20px;margin-bottom:32px;flex-wrap:wrap;}
.wf-title h2{font-size:22px;font-weight:800;color:#0d1f3c;margin:0 0 8px;}
.wf-title p{font-size:13px;color:#6b7a90;margin:0;max-width:660px;line-height:1.65;}
.wf-hint{background:#eff6ff;border:1.5px solid #bfdbfe;border-radius:10px;padding:14px 18px;color:#1e40af;font-size:12px;font-weight:600;line-height:1.8;flex-shrink:0;text-align:center;align-self:flex-start;min-width:150px;}
.wf-flow{display:flex;align-items:stretch;flex-wrap:nowrap;gap:0;margin-bottom:28px;overflow-x:auto;padding-bottom:4px;}
.wf-step{background:#fff;border:1.5px solid #d8dee6;border-radius:12px;padding:18px 16px;flex:1;min-width:140px;max-width:200px;display:flex;flex-direction:column;gap:0;}
.wf-sn{width:26px;height:26px;border-radius:50%;background:#2563eb;color:#fff;font-weight:800;font-size:11px;display:flex;align-items:center;justify-content:center;margin-bottom:10px;flex-shrink:0;}
.wf-si{font-size:24px;margin-bottom:8px;line-height:1;}
.wf-st{font-size:12px;font-weight:700;color:#0d1f3c;margin-bottom:5px;line-height:1.35;}
.wf-sd{font-size:11px;color:#6b7a90;line-height:1.55;flex:1;margin-bottom:10px;}
.wf-sr{display:inline-block;padding:3px 10px;border-radius:999px;font-size:10px;font-weight:700;align-self:flex-start;margin-top:auto;white-space:nowrap;}
.wf-arr{display:flex;align-items:center;justify-content:center;padding:0 6px;color:#94a3b8;font-size:20px;flex:0 0 20px;align-self:center;line-height:1;}
.rb-plan{background:#d1fae5;color:#065f46;}
.rb-eng{background:#dbeafe;color:#1e40af;}
.rb-sr{background:#ede9fe;color:#5b21b6;}
.rb-tl{background:#fef3c7;color:#92400e;}
.rb-mgr{background:#fee2e2;color:#991b1b;}
.rb-sec{background:#fce7f3;color:#9d174d;}
.rb-all{background:#f1f5f9;color:#475569;}
.rb-tbd{background:#f1f5f9;color:#94a3b8;font-style:italic;}
.wf-acc-hdr{font-size:11px;font-weight:700;color:#94a3b8;letter-spacing:.6px;text-transform:uppercase;margin:22px 0 10px;padding-top:22px;border-top:1.5px solid #e5e7eb;}
.wf-acc-grid{display:flex;flex-wrap:wrap;gap:8px;}
.wf-ai{background:#fff;border:1.5px solid #d8dee6;border-radius:8px;padding:11px 15px;flex:1;min-width:175px;}
.wf-ai b{display:block;font-size:12px;color:#0d1f3c;margin-bottom:3px;}
.wf-ai span{font-size:11px;color:#6b7a90;}
@media(max-width:800px){.wf-flow{flex-wrap:wrap;}.wf-arr{transform:rotate(90deg);padding:2px 0;align-self:center;flex:0 0 28px;}.wf-top{flex-direction:column;}.wf-step{min-width:120px;}}
.wf-module{margin-bottom:32px;}
.wf-module-lbl{font-size:13px;font-weight:800;color:#0d1f3c;margin-bottom:12px;display:flex;align-items:center;gap:8px;padding-left:2px;}
.wf-module-lbl::before{content:'';display:inline-block;width:4px;height:16px;background:#2563eb;border-radius:2px;flex-shrink:0;}
.wf-divider{border:none;border-top:1.5px solid #e5e7eb;margin:0 0 28px;}
.wf-freq{display:block;font-size:10px;font-weight:700;color:#2563eb;background:#eff6ff;border:1px solid #bfdbfe;border-radius:4px;padding:3px 8px;margin-top:6px;line-height:1.5;white-space:normal;}
.wf-freq em{font-style:normal;font-weight:400;color:#64748b;}
</style>
</head>
<body>

<!-- Login Wall -->
<div id="login-wall">
  <div class="lw-card">
    <div class="lw-icon">🔒</div>
    <h2>Login Required</h2>
    <p>Please sign in to access the EMT Dashboard.<br>Module workflows and guides are available after login.</p>
  </div>
</div>

<!-- Top nav bar -->
<div class="topbar" id="page-nav-bar">
    <div class="brand">EO Group Dashboard</div>
    <nav>
        <a href="emt.html" class="active">Equipment Status</a>
        <a href="EOT.html">EO</a>
        <a href="team.html" title="Export Maintenance Team">EMT</a>
        <a href="ETS.html">ETS</a>
        <a href="eomm.html">EOM&amp;M</a>
        <a href="hse.html">HSE</a>
        <a href="mom.html">Group Meeting</a>
        <a href="admin.html" id="nav-admin" style="display:none;">Admin</a>
    </nav>
    <div class="spacer"></div>
    <span class="tb-time" id="topbar-time"></span>
    <button class="tb-btn" id="btn-tb-export" style="display:none;" onclick="toggleExportMenu(event)">Export</button>
    <button class="tb-btn" id="btn-tb-entry" style="display:none;" onclick="openBackend()">Data Entry</button>
    <div class="user-pill" id="topbar-user-pill" style="display:none;">
        <span id="topbar-user-label"></span>
        <button onclick="logout()">Logout</button>
    </div>
    <button class="topbar-login-btn" id="topbar-login-btn" onclick="showAuthModal()">Login</button>
</div>

<header>
    <div class="header-left">
        <span class="logo-badge">EXP OPS</span>
        <h1>Equipment Availability <span>| Export Operations</span></h1>
    </div>
    <div class="header-right">
        <div class="timestamp"><span class="live-dot"></span><span id="current-time"></span></div>
        <div style="position:relative;display:inline-block;" id="export-menu-wrap">
            <button class="login-btn" style="background:var(--good);gap:6px;" onclick="toggleExportMenu(event)" title="Export Daily Report">
                <svg width="13" height="13" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"><path d="M21 15v4a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2v-4"/><polyline points="7 10 12 15 17 10"/><line x1="12" y1="15" x2="12" y2="3"/></svg>
                Export
                <svg width="10" height="10" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"><polyline points="6 9 12 15 18 9"/></svg>
            </button>
            <div id="export-dropdown" style="display:none;position:absolute;right:0;top:calc(100% + 6px);background:var(--surface);border:1px solid var(--border);border-radius:var(--radius);box-shadow:var(--shadow-lg);z-index:500;min-width:180px;overflow:hidden;">
                <button onclick="exportDailyReport()" style="display:flex;align-items:center;gap:8px;width:100%;padding:10px 14px;background:none;border:none;font-size:13px;font-weight:500;color:var(--text-primary);cursor:pointer;font-family:'DM Sans',sans-serif;text-align:left;" onmouseover="this.style.background='var(--bg)'" onmouseout="this.style.background='none'">
                    <svg width="13" height="13" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M14 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V8z"/><polyline points="14 2 14 8 20 8"/></svg>
                    Print Daily Report
                </button>
                <button onclick="exportCSV()" style="display:flex;align-items:center;gap:8px;width:100%;padding:10px 14px;background:none;border:none;font-size:13px;font-weight:500;color:var(--text-primary);cursor:pointer;font-family:'DM Sans',sans-serif;text-align:left;" onmouseover="this.style.background='var(--bg)'" onmouseout="this.style.background='none'">
                    <svg width="13" height="13" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M3 3h18v18H3z"/><path d="M3 9h18M3 15h18M9 3v18"/></svg>
                    Download CSV
                </button>
            </div>
        </div>
        <button class="login-btn" id="nav-login-btn" onclick="showAuthModal()">
            <svg width="13" height="13" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"><rect x="3" y="11" width="18" height="11" rx="2"/><path d="M7 11V7a5 5 0 0 1 10 0v4"/></svg>
            Data Entry
        </button>
    </div>
</header>

<main>

<!-- ══════════════════════════════════════════════
     DASHBOARD VIEW (populated by JS on boot)
══════════════════════════════════════════════ -->
<div id="dashboard-view">
    <!-- Content rendered by boot script -->
</div>

<!-- ══════════════════════════════════════════════
     BACKEND VIEW
══════════════════════════════════════════════ -->
<div id="backend-view">
    <div class="backend-header">
        <div>
            <h2>Data Management Portal</h2>
            <div style="margin-top:4px; font-size:12px; color:var(--text-muted);">
                Logged in as: <strong id="current-username" style="color:var(--text-primary);"></strong>
                <span class="role-badge" id="current-role" style="margin-left:6px;"></span>
            </div>
        </div>
        <button class="login-btn" style="background:var(--bg); color:var(--text-secondary); border:1px solid var(--border);" onclick="logout()">
            <svg width="13" height="13" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"><path d="M9 21H5a2 2 0 0 1-2-2V5a2 2 0 0 1 2-2h4"/><polyline points="16 17 21 12 16 7"/><line x1="21" y1="12" x2="9" y2="12"/></svg>
            Logout & Return
        </button>
    </div>

    <!-- Tab Nav -->
    <div class="portal-tabs">
        <button class="ptab active" id="ptab-issues"   onclick="switchTab('issues')">📋 Post Issue</button>
        <button class="ptab"        id="ptab-tankmaint" onclick="switchTab('tankmaint')" style="display:none;">🛢️ Tank on Maint</button>
        <button class="ptab"        id="ptab-mothballed" onclick="switchTab('mothballed')" style="display:none;">📦 Mothballed</button>
        <button class="ptab"        id="ptab-sites"    onclick="switchTab('sites')">📍 Sites</button>
        <button class="ptab"        id="ptab-equip"    onclick="switchTab('equip')">⚙️ Equipment</button>
        <button class="ptab"        id="ptab-status"   onclick="switchTab('status')">🔖 Short Status</button>
        <button class="ptab"        id="ptab-history"  onclick="switchTab('history')" style="display:none;">🕓 History</button>
        <button class="ptab"        id="ptab-admin"    onclick="switchTab('admin')"   style="display:none !important;">🔑 Admin</button>
    </div>

    <!-- ── TAB: POST ISSUE ── -->
    <div class="ptab-panel" id="panel-issues">
        <div class="backend-grid">
            <div class="entry-form-card">
                <h3 id="form-title">Post New Issue</h3>
                <div class="form-group">
                    <label>Department</label>
                    <select id="entry-dept" onchange="updateEntrySiteDropdown()">
                        <option value="">— Select Department —</option>
                        <option value="Electrical">⚡ Electrical</option>
                        <option value="Mechanical">🔧 Mechanical</option>
                        <option value="Instrument">🧪 Instrument</option>
                        <option value="Building Maintenance">🏗 BM</option>
                    </select>
                </div>
                <div class="form-group">
                    <label>Site / Location</label>
                    <select id="entry-site" onchange="filterEquipBysite()"></select>
                </div>
                <div class="form-group">
                    <label>Select Equipment <span style="color:var(--accent);font-size:10px;font-weight:600;">(from master list)</span></label>
                    <select id="entry-equip-select" onchange="autofillEquip()">
                        <option value="">— Choose equipment —</option>
                    </select>
                </div>
                <div style="display:flex; gap:10px;">
                    <div class="form-group" style="flex:1;"><label>Tag / Asset ID</label><input type="text" id="entry-id" placeholder="Auto-filled or enter manually"></div>
                    <div class="form-group" style="flex:2;"><label>Equipment Name</label><input type="text" id="entry-name" placeholder="Auto-filled or enter manually"></div>
                </div>
                <div style="display:flex; gap:10px;">
                    <div class="form-group" style="flex:1;"><label>Status Level</label><select id="entry-level"><option value="crit">Critical (Red)</option><option value="warn">Warning (Amber)</option></select></div>
                    <div class="form-group" style="flex:1;"><label>Short Status</label>
                        <select id="entry-status">
                            <option value="">— Select status —</option>
                        </select>
                    </div>
                </div>
                <div class="form-group"><label>Reason / Issue Description</label><textarea id="entry-issue" rows="2" placeholder="Describe the issue in detail..."></textarea></div>
                <div class="form-group"><label>Action Taken</label><textarea id="entry-action" rows="2" placeholder="Steps taken or planned..."></textarea></div>
                <div style="display:flex; gap:10px;">
                    <div class="form-group" style="flex:1;"><label>Remarks</label><input type="text" id="entry-remarks" placeholder="SR numbers, notes..."></div>
                    <div class="form-group" style="flex:1;"><label>Expected Return Date</label><input type="date" id="entry-etr"></div>
                </div>
                <button class="btn-primary" onclick="submitNewIssue()">Submit Issue to Dashboard</button>
            </div>
            <div class="data-table-card">
                <h3>Current Active Records</h3>
                <table class="data-table">
                    <thead><tr><th>Asset ID</th><th>Site</th><th>Department</th><th>Status</th><th>Posted By</th><th>Action</th></tr></thead>
                    <tbody id="backend-table-body"></tbody>
                </table>
            </div>
        </div>
    </div>

    <!-- ── TAB: TANK ON MAINT ── -->
    <div class="ptab-panel" id="panel-tankmaint" style="display:none;">
        <div class="backend-grid">
            <div class="entry-form-card">
                <h3 id="tank-form-title">Add Tank Maintenance Record</h3>
                <div class="form-group"><label>Tank Number</label><input type="text" id="tank-no" placeholder="e.g. Tank 71"></div>
                <div class="form-group"><label>SR Number</label><input type="text" id="tank-sr-no" placeholder="e.g. EOT/I-3/433/25"></div>
                <div style="display:flex; gap:10px;">
                    <div class="form-group" style="flex:1;"><label>Date of Handing Over</label><input type="date" id="tank-handover-date"></div>
                    <div class="form-group" style="flex:1;"><label>Expected Date of Return (EDR)</label><input type="date" id="tank-edr"></div>
                </div>
                <div style="display:flex; gap:10px;">
                    <div class="form-group" style="flex:1;"><label>Status</label>
                        <select id="tank-status">
                            <option value="Out of Commission">Out of Commission</option>
                            <option value="Returned">Returned</option>
                        </select>
                    </div>
                    <div class="form-group" style="flex:1;"><label>Actual Return Date</label><input type="date" id="tank-return-date"></div>
                </div>
                <div class="form-group"><label>Remarks</label><textarea id="tank-remarks" rows="3" placeholder="Work scope, notes, or latest EO update..."></textarea></div>
                <button class="btn-primary" onclick="submitTankMaintenance()">Save Tank Record</button>
                <button class="btn-secondary" id="tank-cancel-edit-btn" onclick="cancelTankEdit()" style="display:none;margin-top:6px;">Cancel Edit</button>
            </div>
            <div class="data-table-card">
                <div style="display:flex;justify-content:space-between;align-items:center;gap:12px;flex-wrap:wrap;">
                    <h3>Tank Maintenance Register</h3>
                    <select id="tank-table-filter" onchange="renderTankBackendTable()" style="padding:7px 10px;border:1px solid var(--border);border-radius:5px;font-size:12px;font-family:'DM Sans',sans-serif;">
                        <option value="active">Active Only</option>
                        <option value="returned">Returned Only</option>
                        <option value="all">All Records</option>
                    </select>
                </div>
                <div style="max-height:440px;overflow-y:auto;margin-top:10px;">
                    <table class="data-table">
                        <thead><tr><th>Tank</th><th>SR No.</th><th>Handed Over</th><th>EDR</th><th>Status</th><th>Return Date</th><th>Action</th></tr></thead>
                        <tbody id="tank-backend-table-body"></tbody>
                    </table>
                </div>
            </div>
        </div>
    </div>

    <!-- ── TAB: MOTHBALLED EQUIPMENT ── -->
    <div class="ptab-panel" id="panel-mothballed" style="display:none;">
        <div class="backend-grid">
            <div class="entry-form-card">
                <h3 id="mothballed-form-title">Add Mothballed Equipment Record</h3>
                <div class="form-group"><label>Asset Name</label><input type="text" id="mothballed-asset-name" placeholder="e.g. CRUDE EXPORT PUMP 612-G-23"></div>
                <div class="form-group"><label>Equipment Tag</label><input type="text" id="mothballed-equipment-tag" placeholder="e.g. 612-G-23"></div>
                <div style="display:flex; gap:10px;">
                    <div class="form-group" style="flex:1;"><label>Site</label><input type="text" id="mothballed-site" placeholder="e.g. NPP"></div>
                    <div class="form-group" style="flex:1;"><label>Department</label>
                        <input type="text" id="mothballed-dept" list="mothballed-dept-options" placeholder="e.g. Mechanical">
                        <datalist id="mothballed-dept-options">
                            <option value="Mechanical"></option>
                            <option value="Electrical"></option>
                            <option value="Instrument"></option>
                            <option value="Building Maintenance"></option>
                        </datalist>
                    </div>
                </div>
                <div style="display:flex; gap:10px;">
                    <div class="form-group" style="flex:1;"><label>Mothballed On</label><input type="date" id="mothballed-on"></div>
                    <div class="form-group" style="flex:1;"><label>Mothballed By</label><input type="text" id="mothballed-by" placeholder="e.g. EO"></div>
                </div>
                <div style="display:flex; gap:10px;">
                    <div class="form-group" style="flex:1;"><label>Status</label>
                        <select id="mothballed-status">
                            <option value="Mothballed">Mothballed</option>
                            <option value="Reactivated">Reactivated</option>
                        </select>
                    </div>
                    <div class="form-group" style="flex:1;"><label>Reactivated On</label><input type="date" id="mothballed-reactivated-on"></div>
                </div>
                <div class="form-group"><label>Reactivated By</label><input type="text" id="mothballed-reactivated-by" placeholder="e.g. EO / Mechanical"></div>
                <div class="form-group"><label>Future Plan / Remarks</label><textarea id="mothballed-future-plan" rows="3" placeholder="Preservation plan, replacement strategy, remarks, or return-to-service notes..."></textarea></div>
                <button class="btn-primary" onclick="submitMothballedEquipment()">Save Mothballed Record</button>
                <button class="btn-secondary" id="mothballed-cancel-edit-btn" onclick="cancelMothballedEdit()" style="display:none;margin-top:6px;">Cancel Edit</button>
            </div>
            <div class="data-table-card">
                <div style="display:flex;justify-content:space-between;align-items:center;gap:12px;flex-wrap:wrap;">
                    <h3>Mothballed Equipment Register</h3>
                    <select id="mothballed-table-filter" onchange="renderMothballedBackendTable()" style="padding:7px 10px;border:1px solid var(--border);border-radius:5px;font-size:12px;font-family:'DM Sans',sans-serif;">
                        <option value="active">Active Only</option>
                        <option value="reactivated">Reactivated Only</option>
                        <option value="all">All Records</option>
                    </select>
                </div>
                <div style="max-height:440px;overflow-y:auto;margin-top:10px;">
                    <table class="data-table">
                        <thead><tr><th>Asset</th><th>Tag</th><th>Mothballed On</th><th>By</th><th>Status</th><th>Reactivated</th><th>Action</th></tr></thead>
                        <tbody id="mothballed-backend-table-body"></tbody>
                    </table>
                </div>
            </div>
        </div>
    </div>

    <!-- ── TAB: SITES ── -->
    <div class="ptab-panel" id="panel-sites" style="display:none;">
        <div class="backend-grid">
            <div class="entry-form-card">
                <h3>Add / Edit Site</h3>
                <div class="form-group"><label>Site ID</label><input type="text" id="admin-site-id" placeholder="e.g. STF"></div>
                <div class="form-group"><label>Icon (emoji)</label><input type="text" id="admin-site-icon" placeholder="🏭"></div>
                <div style="display:flex;gap:10px;">
                    <div class="form-group" style="flex:1;"><label>Elec Count</label><input type="number" id="admin-site-elec" value="0"></div>
                    <div class="form-group" style="flex:1;"><label>Mech Count</label><input type="number" id="admin-site-mech" value="0"></div>
                    <div class="form-group" style="flex:1;"><label>Inst Count</label><input type="number" id="admin-site-inst" value="0"></div>
                </div>
                <button class="btn-primary" onclick="adminSaveSite()">Save Site</button>
            </div>
            <div class="data-table-card">
                <h3>All Sites</h3>
                <div style="max-height:400px;overflow-y:auto;">
                    <table class="data-table">
                        <thead><tr><th>ID</th><th>Icon</th><th>Elec</th><th>Mech</th><th>Inst</th><th>Actions</th></tr></thead>
                        <tbody id="admin-sites-table"></tbody>
                    </table>
                </div>
            </div>
        </div>
    </div>

    <!-- ── TAB: EQUIPMENT ── -->
    <div class="ptab-panel" id="panel-equip" style="display:none;">
        <div class="backend-grid">
            <div class="entry-form-card">
                <h3>Add / Edit Equipment</h3>
                <div class="form-group"><label>Equipment Name</label><input type="text" id="eq-name" placeholder="e.g. EFFLUENT PUMP-A"></div>
                <div class="form-group"><label>Tag / Asset ID</label><input type="text" id="eq-tag" placeholder="e.g. P-601A"></div>
                <div class="form-group"><label>Department</label>
                    <select id="eq-dept">
                        <option value="Mechanical">🔧 Mechanical</option>
                        <option value="Electrical">⚡ Electrical</option>
                        <option value="Instrument">🧪 Instrument</option>
                    </select>
                </div>
                <div class="form-group"><label>Site</label>
                    <select id="eq-site"></select>
                </div>
                <input type="hidden" id="eq-edit-idx" value="">
                <button class="btn-primary" onclick="saveEquipment()">Save Equipment</button>
                <button class="btn-secondary" id="eq-cancel-btn" onclick="cancelEquipEdit()" style="display:none;margin-top:6px;">Cancel Edit</button>
            </div>
            <div class="data-table-card">
                <h3>Equipment Master List <span id="eq-count" style="color:var(--text-muted);font-size:12px;font-weight:400;"></span></h3>
                <div style="display:flex;gap:8px;margin-bottom:10px;">
                    <input type="text" id="eq-search" placeholder="Search name or tag..." oninput="renderEquipTable()" style="flex:1;padding:7px 10px;border:1px solid var(--border);border-radius:5px;font-size:12px;font-family:'DM Sans',sans-serif;">
                    <select id="eq-filter-dept" onchange="renderEquipTable()" style="padding:7px 10px;border:1px solid var(--border);border-radius:5px;font-size:12px;font-family:'DM Sans',sans-serif;">
                        <option value="">All Depts</option>
                        <option value="Electrical">Electrical</option>
                        <option value="Mechanical">Mechanical</option>
                        <option value="Instrument">Instrument</option>
                    </select>
                    <select id="eq-filter-site" onchange="renderEquipTable()" style="padding:7px 10px;border:1px solid var(--border);border-radius:5px;font-size:12px;font-family:'DM Sans',sans-serif;">
                        <option value="">All Sites</option>
                    </select>
                </div>
                <div style="max-height:400px;overflow-y:auto;">
                    <table class="data-table">
                        <thead><tr><th>Tag</th><th>Name</th><th>Dept</th><th>Site</th><th>Source</th><th>Actions</th></tr></thead>
                        <tbody id="equip-table-body"></tbody>
                    </table>
                </div>
            </div>
        </div>
    </div>

    <!-- ── TAB: SHORT STATUS ── -->
    <div class="ptab-panel" id="panel-status" style="display:none;">
        <div class="backend-grid">
            <div class="entry-form-card">
                <h3>Add / Edit Short Status</h3>
                <div class="form-group"><label>Status Label</label><input type="text" id="ss-label" placeholder="e.g. Bearing Failure"></div>
                <div class="form-group"><label>Applies To</label>
                    <select id="ss-dept">
                        <option value="All">All Departments</option>
                        <option value="Mechanical">🔧 Mechanical</option>
                        <option value="Electrical">⚡ Electrical</option>
                        <option value="Instrument">🧪 Instrument</option>
                    </select>
                </div>
                <input type="hidden" id="ss-edit-idx" value="">
                <button class="btn-primary" onclick="saveShortStatus()">Save Status</button>
                <button class="btn-secondary" id="ss-cancel-btn" onclick="cancelSSEdit()" style="display:none;margin-top:6px;">Cancel Edit</button>
            </div>
            <div class="data-table-card">
                <h3>Short Status List</h3>
                <div style="max-height:400px;overflow-y:auto;">
                    <table class="data-table">
                        <thead><tr><th>Label</th><th>Dept</th><th>Actions</th></tr></thead>
                        <tbody id="status-table-body"></tbody>
                    </table>
                </div>
            </div>
        </div>
    </div>

    <!-- ── TAB: HISTORY (Admin only) ── -->
    <div class="ptab-panel" id="panel-history" style="display:none;">
        <div class="data-table-card" style="max-width:100%;">
            <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:12px;">
                <h3 style="margin:0;">Audit Log — All Actions</h3>
                <button class="btn-danger" onclick="if(confirm('Clear the displayed log? (Server records are retained — this only clears your current view.)')){auditLog=[];renderAuditLog();}">Clear View</button>
            </div>
            <div style="display:flex;gap:8px;margin-bottom:10px;">
                <select id="hist-filter-user" onchange="renderAuditLog()" style="padding:7px 10px;border:1px solid var(--border);border-radius:5px;font-size:12px;font-family:'DM Sans',sans-serif;">
                    <option value="">All Users</option>
                </select>
                <select id="hist-filter-action" onchange="renderAuditLog()" style="padding:7px 10px;border:1px solid var(--border);border-radius:5px;font-size:12px;font-family:'DM Sans',sans-serif;">
                    <option value="">All Actions</option>
                    <option value="ADD_ISSUE">Add Issue</option>
                    <option value="RESOLVE_ISSUE">Resolve Issue</option>
                    <option value="ADD_SITE">Add Site</option>
                    <option value="EDIT_SITE">Edit Site</option>
                    <option value="DEL_SITE">Delete Site</option>
                    <option value="ADD_EQUIP">Add Equipment</option>
                    <option value="EDIT_EQUIP">Edit Equipment</option>
                    <option value="DEL_EQUIP">Delete Equipment</option>
                    <option value="ADD_STATUS">Add Status</option>
                    <option value="DEL_STATUS">Delete Status</option>
                </select>
            </div>
            <div style="max-height:500px;overflow-y:auto;">
                <table class="data-table">
                    <thead><tr><th>Date / Time</th><th>User</th><th>Role</th><th>Action</th><th>Detail</th></tr></thead>
                    <tbody id="audit-table-body"></tbody>
                </table>
            </div>
        </div>
    </div>

    <!-- ── TAB: ADMIN (Admin only) ── -->
    <div class="ptab-panel" id="panel-admin" style="display:none;">
        <div class="backend-grid">
            <!-- Create / Edit User -->
            <div class="entry-form-card">
                <h3 id="admin-form-title">Create New User</h3>
                <input type="hidden" id="admin-edit-user">
                <div class="form-group"><label>Username</label><input type="text" id="admin-username" placeholder="e.g. john_elec"></div>
                <div class="form-group"><label>Password <span id="admin-pass-hint" style="font-size:10px;color:var(--text-muted);">(required for new user)</span></label>
                    <input type="text" id="admin-new-pass" placeholder="Min 4 characters"></div>
                <div class="form-group"><label>Role / Discipline</label>
                    <select id="admin-role" onchange="applyRoleDefaults()">
                        <optgroup label="── EMT (Export Maintenance Team) ──">
                            <option value="TLEMT">👷 TL EMT (Team Leader)</option>
                            <option value="Electrical">⚡ Electrical</option>
                            <option value="Mechanical">🔧 Mechanical</option>
                            <option value="Instrument">🧪 Instrument</option>
                            <option value="Building Maintenance">🏗️ BM (Building Maintenance)</option>
                        </optgroup>
                        <optgroup label="── Management ──">
                            <option value="MEO">🏛️ MEO</option>
                        </optgroup>
                        <optgroup label="── Group Meeting ──">
                            <option value="Planner">📋 Planner (MOM Editor)</option>
                            <option value="EO">🏢 EO</option>
                            <option value="EOT">🛢️ EOT</option>
                            <option value="ETS">⚗️ ETS</option>
                            <option value="OilMov">🚢 Oil Movement</option>
                            <option value="HSE">🦺 HSE</option>
                        </optgroup>
                    </select>
                </div>
                <div class="form-group">
                    <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:6px;">
                        <label style="margin:0;">Privileges</label>
                        <button type="button" onclick="applyRoleDefaults()" style="font-size:11px;padding:3px 10px;background:var(--accent-light);color:var(--accent);border:1px solid var(--accent);border-radius:4px;cursor:pointer;font-family:inherit;font-weight:600;">↺ Reset to Role Defaults</button>
                    </div>
                    <div id="perm-matrix-wrap" style="max-height:360px;overflow-y:auto;border:1px solid var(--border);border-radius:6px;"></div>
                </div>
                <button class="btn-primary" onclick="adminSaveUser()">Create User</button>
                <button class="btn-secondary" id="admin-cancel-edit" onclick="adminCancelEdit()" style="display:none;margin-top:6px;">Cancel Edit</button>
                <div id="admin-msg" style="color:var(--good); margin-top:8px; font-size:12px; font-weight:600;"></div>

                <hr style="border:none;border-top:1px solid var(--border);margin:20px 0;">
                <h3>Change Password</h3>
                <div style="display:flex; gap:10px; align-items:flex-end;">
                    <div class="form-group" style="margin:0;flex:1;"><label>Select User</label><select id="admin-user-select"></select></div>
                    <div class="form-group" style="margin:0;flex:1;"><label>New Password</label><input type="text" id="admin-chg-pass" placeholder="Min 4 characters"></div>
                    <button class="btn-primary" style="width:auto;margin:0;padding:8px 14px;" onclick="adminChangePassword()">Update</button>
                </div>
                <div id="admin-pass-msg" style="color:var(--good); margin-top:8px; font-size:12px; font-weight:600;"></div>
            </div>

            <!-- User List -->
            <div class="data-table-card">
                <h3>User Accounts</h3>
                <div style="max-height:600px;overflow-y:auto;">
                    <table class="data-table">
                        <thead><tr><th>Username</th><th>Role</th><th>Page Access (granted privileges)</th><th>Actions</th></tr></thead>
                        <tbody id="admin-users-table"></tbody>
                    </table>
                </div>
            </div>
        </div>
    </div>

</div>

</main>

<!-- AUTH MODAL -->
<div class="auth-overlay" id="auth-modal">
    <div class="auth-box">
        <h2>System Login</h2>
        <p>Enter your credentials to access the data portal.</p>
        <div class="form-group"><label>Username</label><input type="text" id="auth-user" placeholder="admin, elec, mech, inst, BM"></div>
        <div class="form-group"><label>Password</label><input type="password" id="auth-pass"></div>
        <div class="auth-error" id="auth-error">Invalid credentials. Please try again.</div>
        <button class="btn-primary" onclick="processLogin()">Login</button>
        <button class="btn-secondary" onclick="closeAuthModal()">Cancel</button>
    </div>
</div>

<!-- ITEM MODAL -->
<div class="modal-overlay" id="item-modal" onclick="closeModal(event)">
    <div class="modal-content" onclick="event.stopPropagation()">
        <div class="modal-header">
            <div>
                <h2 id="modal-equip-name">Equipment Name</h2>
                <span class="sub-text" id="modal-equip-id">Asset ID</span>
            </div>
            <button class="close-btn" onclick="closeModal(true)">✕</button>
        </div>
        <div style="display:grid; grid-template-columns:1fr 1fr 1fr; gap:14px;">
            <div class="detail-row"><label>Department</label><div class="detail-value" id="modal-dept"></div></div>
            <div class="detail-row"><label>Current Status</label><div class="detail-value" id="modal-status" style="font-weight:700;"></div></div>
            <div class="detail-row"><label>Expected Return</label><div class="detail-value" id="modal-etr" style="font-family:'DM Mono',monospace;"></div></div>
            <div class="detail-row" style="grid-column:1/span 3;"><label>Failure Date</label><div class="detail-value" id="modal-date" style="font-family:'DM Mono',monospace;"></div></div>
        </div>
        <div class="detail-row"><label>Reason / Issue Description</label><div class="detail-value" id="modal-update"></div></div>
        <div class="detail-row"><label>Action Plan / Corrective Steps</label><div class="detail-value" id="modal-action"></div></div>
        <div class="detail-row"><label>Additional Remarks</label><div class="detail-value" id="modal-remarks"></div></div>
    </div>
</div>

<script>
    // ─── TIME ───
    function updateTime() {
        const t = new Date().toLocaleString('en-GB', { day:'2-digit', month:'short', year:'numeric', hour:'2-digit', minute:'2-digit' });
        const el = document.getElementById('current-time');
        if (el) el.innerText = t;
        const tb = document.getElementById('topbar-time');
        if (tb) tb.innerHTML = '&#9679; ' + t;
    }
    setInterval(updateTime, 30000); updateTime();

    // ═══════════════════════════════════════════════════════════
    // MASTER EQUIPMENT DATABASE  (sourced from Equipment_List.xlsx)
    // ═══════════════════════════════════════════════════════════
    const masterEquipmentDb = [
      // ── MECHANICAL – P&MS ──
      {name:"KEC PUMP-A",                              tag:"611-G-800A",         site:"P&MS",     dept:"Mechanical"},
      {name:"KEC PUMP-B",                              tag:"611-G-800B",         site:"P&MS",     dept:"Mechanical"},
      {name:"KEC PUMP-C",                              tag:"611-G-800C",         site:"P&MS",     dept:"Mechanical"},
      {name:"KMH PUMP-A",                              tag:"611-G-801A",         site:"P&MS",     dept:"Mechanical"},
      {name:"KMH PUMP-B",                              tag:"611-G-801B",         site:"P&MS",     dept:"Mechanical"},
      {name:"KMH PUMP-C",                              tag:"611-G-801C",         site:"P&MS",     dept:"Mechanical"},
      {name:"EOCENE PUMP-A",                           tag:"611-G-802A",         site:"P&MS",     dept:"Mechanical"},
      {name:"EOCENE PUMP-B",                           tag:"611-G-802B",         site:"P&MS",     dept:"Mechanical"},
      {name:"LFHO PUMP A",                             tag:"611-G-803A",         site:"P&MS",     dept:"Mechanical"},
      {name:"LFHO PUMP B",                             tag:"611-G-803B",         site:"P&MS",     dept:"Mechanical"},
      {name:"LFHO PUMP C",                             tag:"611-G-803C",         site:"P&MS",     dept:"Mechanical"},
      {name:"JOCKEY WATER PUMP A",                     tag:"611-G-813A",         site:"P&MS",     dept:"Mechanical"},
      {name:"JOCKEY WATER PUMP B",                     tag:"611-G-813B",         site:"P&MS",     dept:"Mechanical"},
      {name:"FIRE WATER PUMP-A",                       tag:"611-G-814A",         site:"P&MS",     dept:"Mechanical"},
      {name:"FIRE WATER PUMP-B",                       tag:"611-G-814B",         site:"P&MS",     dept:"Mechanical"},
      {name:"AIR COMPRESSOR-A",                        tag:"611-K-1810",         site:"P&MS",     dept:"Mechanical"},
      {name:"AIR COMPRESSOR-B",                        tag:"611-K-2810",         site:"P&MS",     dept:"Mechanical"},
      // ── MECHANICAL – STF ──
      {name:"EFFLUENT PUMP-A",                         tag:"P-601A",             site:"STF",      dept:"Mechanical"},
      {name:"EFFLUENT PUMP-B",                         tag:"P-601B",             site:"STF",      dept:"Mechanical"},
      {name:"FIRE WATER PUMP-201 (Cummins)",           tag:"FP-201",             site:"STF",      dept:"Mechanical"},
      {name:"FIRE WATER PUMP-202 (Cummins)",           tag:"FP-202",             site:"STF",      dept:"Mechanical"},
      {name:"FIRE WATER PUMP-203 (Cummins)",           tag:"FP-203",             site:"STF",      dept:"Mechanical"},
      {name:"FIRE WATER PUMP-204 (Jockey)",            tag:"FP-204",             site:"STF",      dept:"Mechanical"},
      {name:"FIRE WATER PUMP-205 (Jockey)",            tag:"FP-205",             site:"STF",      dept:"Mechanical"},
      {name:"FIRE WATER PUMP-206 (Jockey)",            tag:"FP-206",             site:"STF",      dept:"Mechanical"},
      {name:"FIRE WATER PUMP-207 (Caterpillar)",       tag:"FP-207",             site:"STF",      dept:"Mechanical"},
      {name:"FIRE WATER PUMP-208 (Caterpillar)",       tag:"FP-208",             site:"STF",      dept:"Mechanical"},
      // ── MECHANICAL – NTF ──
      {name:"EFFLUENT WATER PUMP A",                   tag:"620-G-601A",         site:"NTF",      dept:"Mechanical"},
      {name:"EFFLUENT WATER PUMP B",                   tag:"620-G-601B",         site:"NTF",      dept:"Mechanical"},
      {name:"EFFLUENT TRANSFER PUMP S6-A",             tag:"270-G-8A",           site:"NTF",      dept:"Mechanical"},
      {name:"EFFLUENT TRANSFER PUMP S6-B",             tag:"270-G-8B",           site:"NTF",      dept:"Mechanical"},
      {name:"FIRE WATER PUMP-101 (Cummins)",           tag:"FP-101",             site:"NTF",      dept:"Mechanical"},
      {name:"FIRE WATER PUMP-102 (Cummins)",           tag:"FP-102",             site:"NTF",      dept:"Mechanical"},
      {name:"FIRE WATER PUMP-103 (Cummins)",           tag:"FP-103",             site:"NTF",      dept:"Mechanical"},
      {name:"FIRE WATER PUMP-104 (Jockey)",            tag:"FP-104",             site:"NTF",      dept:"Mechanical"},
      {name:"FIRE WATER PUMP-105 (Jockey)",            tag:"FP-105",             site:"NTF",      dept:"Mechanical"},
      {name:"FIRE WATER PUMP-106 (Caterpillar)",       tag:"FP-106",             site:"NTF",      dept:"Mechanical"},
      // ── MECHANICAL – SPH (Water Section) ──
      {name:"FRESH WATER PUMP-A (Supervisory)",        tag:"PUMP-A",             site:"SPH",      dept:"Mechanical"},
      {name:"FRESH WATER PUMP-B NEW (Supervisory)",    tag:"PUMP-B NEW",         site:"SPH",      dept:"Mechanical"},
      {name:"FRESH WATER PUMP-C (Supervisory)",        tag:"PUMP-C",             site:"SPH",      dept:"Mechanical"},
      {name:"FRESH WATER PUMP-D NEW (Supervisory)",    tag:"PUMP-D NEW",         site:"SPH",      dept:"Mechanical"},
      // ── MECHANICAL – STF-APH ──
      {name:"FRESH WATER PUMP D - Ahmadi PH",          tag:"G-4001D",            site:"STF-APH",  dept:"Mechanical"},
      {name:"FRESH WATER PUMP C - Ahmadi PH",          tag:"G-4001C",            site:"STF-APH",  dept:"Mechanical"},
      {name:"FRESH WATER PUMP B - Ahmadi PH",          tag:"G-4001B",            site:"STF-APH",  dept:"Mechanical"},
      {name:"FRESH WATER PUMP A Diesel - Ahmadi PH",   tag:"G-4001-A",           site:"STF-APH",  dept:"Mechanical"},
      {name:"SPRINKLER FIRE PUMP",                     tag:"G-4003",             site:"STF-APH",  dept:"Mechanical"},
      {name:"BRACKISH WATER PUMP A - Ahmadi PH",       tag:"G-4002A",            site:"STF-APH",  dept:"Mechanical"},
      {name:"BRACKISH WATER PUMP B - Ahmadi PH",       tag:"G-4002B",            site:"STF-APH",  dept:"Mechanical"},
      {name:"BRACKISH WATER PUMP C - Ahmadi PH",       tag:"G-4002C",            site:"STF-APH",  dept:"Mechanical"},
      {name:"BRACKISH WATER PUMP D - Ahmadi PH",       tag:"G-4002D",            site:"STF-APH",  dept:"Mechanical"},
      {name:"DESALTER PUMP A",                         tag:"621-P-LCS A",        site:"STF-APH",  dept:"Mechanical"},
      {name:"DESALTER PUMP B",                         tag:"621-P-LCS B",        site:"STF-APH",  dept:"Mechanical"},
      {name:"DESALTER PUMP C",                         tag:"621-P-LCS C",        site:"STF-APH",  dept:"Mechanical"},
      {name:"DESALTER PUMP ST-HS-050-1",               tag:"ST-HS-050-1",        site:"STF-APH",  dept:"Mechanical"},
      {name:"DESALTER PUMP ST-HS-050-2",               tag:"ST-HS-050-2",        site:"STF-APH",  dept:"Mechanical"},
      {name:"DESALTER PUMP ST-HS-050-3",               tag:"ST-HS-050-3",        site:"STF-APH",  dept:"Mechanical"},
      {name:"TK8&9 TRANSFER PUMP JB-1",                tag:"JB-1",               site:"STF-APH",  dept:"Mechanical"},
      {name:"TK8&9 TRANSFER PUMP JB-2",                tag:"JB-2",               site:"STF-APH",  dept:"Mechanical"},
      {name:"TK8&9 TRANSFER PUMP JB-3",                tag:"JB-3",               site:"STF-APH",  dept:"Mechanical"},
      // ── MECHANICAL – NPP ──
      {name:"CRUDE EXPORT PUMP 612-G-20",              tag:"612-G-20",           site:"NPP",      dept:"Mechanical"},
      {name:"CRUDE EXPORT PUMP 612-G-23",              tag:"612-G-23",           site:"NPP",      dept:"Mechanical"},
      {name:"CRUDE EXPORT PUMP 612-G-24",              tag:"612-G-24",           site:"NPP",      dept:"Mechanical"},
      {name:"PURGE AIR COMPRESSOR A",                  tag:"612-K-28A",          site:"NPP",      dept:"Mechanical"},
      {name:"PURGE AIR COMPRESSOR B",                  tag:"612-K-28B",          site:"NPP",      dept:"Mechanical"},
      {name:"FIRE WATER MAIN PUMP A",                  tag:"612-G-007A",         site:"NPP",      dept:"Mechanical"},
      {name:"FIRE WATER MAIN PUMP B",                  tag:"612-G-007B",         site:"NPP",      dept:"Mechanical"},
      {name:"JOCKEY PUMP A",                           tag:"612-G-008A",         site:"NPP",      dept:"Mechanical"},
      {name:"JOCKEY PUMP B",                           tag:"612-G-008B",         site:"NPP",      dept:"Mechanical"},
      // ── MECHANICAL – MINA ──
      {name:"CRUDE EXPORT PUMP P101",                  tag:"P101",               site:"MINA",     dept:"Mechanical"},
      {name:"CRUDE EXPORT PUMP P102",                  tag:"P102",               site:"MINA",     dept:"Mechanical"},

      // ── ELECTRICAL – Emergency Generators ──
      {name:"EMERG. GENERATOR – North Pier Pumping",   tag:"612-EDG-001",        site:"NPP",      dept:"Electrical"},
      {name:"EMERG. GENERATOR – STF FF",               tag:"279-EDG-091",        site:"STF",      dept:"Electrical"},
      {name:"EMERG. GENERATOR – STF A32A",             tag:"279-EDG-092",        site:"STF",      dept:"Electrical"},
      {name:"EMERG. GENERATOR – NTF New Elevated",     tag:"270-EDG-091",        site:"NTF",      dept:"Electrical"},
      {name:"EMERG. GENERATOR – NTF RMU#4",            tag:"E286-5001",          site:"NTF",      dept:"Electrical"},
      {name:"EMERG. GENERATOR – NTF RMU#5",            tag:"E286-5002",          site:"NTF",      dept:"Electrical"},
      {name:"EMERG. GENERATOR – NTF RMU#6",            tag:"E286-5003",          site:"NTF",      dept:"Electrical"},
      {name:"EMERG. GENERATOR – NTF RMU#7",            tag:"E286-J004",          site:"NTF",      dept:"Electrical"},
      {name:"EMERG. GENERATOR – ADM",                  tag:"E282-J001",          site:"ADM",      dept:"Electrical"},
      {name:"EMERG. GENERATOR – CMM",                  tag:"281-J001",           site:"CMM",      dept:"Electrical"},
      {name:"EMERG. GENERATOR – CMM 301 S/S",          tag:"621-EDG-501",        site:"CMM",      dept:"Electrical"},
      {name:"EMERG. GENERATOR – STF 302 S/S",          tag:"621-EDG-502",        site:"STF",      dept:"Electrical"},
      {name:"EMERG. GENERATOR – STF 303 S/S",          tag:"621-EDG-503",        site:"STF",      dept:"Electrical"},
      {name:"EMERG. GENERATOR – COCC 001 S/S",         tag:"621-EDG-504",        site:"COCC",     dept:"Electrical"},
      {name:"EMERG. GENERATOR – NRP P&MS",             tag:"NRP CRUDE SS",       site:"P&MS",     dept:"Electrical"},
      // ── ELECTRICAL – Standby Generators ──
      {name:"STANDBY GEN – COCC Old Control Bldg",     tag:"621-SDG-COCC-OLD",   site:"COCC",     dept:"Electrical"},
      {name:"STANDBY GEN – COCC Lab",                  tag:"621-SDG-COCC-LAB",   site:"COCC",     dept:"Electrical"},
      {name:"STANDBY GEN – KNPC M/F",                  tag:"611-SDG-091",        site:"KNPC MF",  dept:"Electrical"},
      {name:"STANDBY GEN – MINA CPH",                  tag:"350-SDG-091",        site:"MINA",     dept:"Electrical"},
      {name:"STANDBY GEN – STF Near Gas Manifold",     tag:"621-SDG-001",        site:"STF",      dept:"Electrical"},
      {name:"STANDBY GEN – STF Near C3 Building",      tag:"621-SDG-002",        site:"STF",      dept:"Electrical"},
      {name:"STANDBY GEN – STF Near RMU#4 (1)",        tag:"621-SDG-003",        site:"STF",      dept:"Electrical"},
      {name:"STANDBY GEN – STF Near RMU#4 (2)",        tag:"621-SDG-004",        site:"STF",      dept:"Electrical"},
      {name:"STANDBY GEN – STF Security Gate",         tag:"621-SDG-005",        site:"STF",      dept:"Electrical"},
      {name:"STANDBY GEN – STF C3 Building",           tag:"621-SDG-006",        site:"STF",      dept:"Electrical"},
      {name:"STANDBY GEN – KNPC Manifold Security",    tag:"611-SDG-KNPC",       site:"KNPC MF",  dept:"Electrical"},
      {name:"SPARE GENERATOR 100 KVA",                 tag:"SPARE-GEN-100KVA",   site:"CMM",      dept:"Electrical"},
      {name:"SPARE GENERATOR 1275 KVA",                tag:"SPARE-GEN-1275KVA",  site:"CMM",      dept:"Electrical"},

      // ── INSTRUMENT – Systems ──
      {name:"Fire & Gas PLC System",                   tag:"F&G-PLC",            site:"ALL",      dept:"Instrument"},
      {name:"Fire & Gas Detectors",                    tag:"F&G-DET",            site:"ALL",      dept:"Instrument"},
      {name:"Fire Alarm Panel",                        tag:"FAP",                site:"ALL",      dept:"Instrument"},
      {name:"ESD System",                              tag:"ESD",                site:"ALL",      dept:"Instrument"},
      {name:"DCS System",                              tag:"DCS",                site:"ALL",      dept:"Instrument"},
      {name:"Tank Gauging System",                     tag:"TANK-GAUGE",         site:"ALL",      dept:"Instrument"},
      {name:"Flow Computer (Metering)",                tag:"FLOW-COMP",          site:"ALL",      dept:"Instrument"},
      {name:"SCADA System",                            tag:"SCADA",              site:"ALL",      dept:"Instrument"},
      {name:"Loading Status – NPP",                    tag:"LOAD-NPP",           site:"NPP",      dept:"Instrument"},
      {name:"Loading Status – CPH",                    tag:"LOAD-CPH",           site:"MINA",     dept:"Instrument"},
      {name:"Loading Status GL3/KEC/LFHO",             tag:"LOAD-KNPC",          site:"KNPC MF",  dept:"Instrument"},
    ];

    // ── SITE ICONS MAP ──
    const siteIcons = {
      'P&MS':'⚡', 'STF':'🏭', 'NTF':'🏢', 'COCC':'🎛️', 'CMM':'⚙️',
      'ADM':'🏛️', 'NPP':'🚢', 'MINA':'⛴️', 'SPH':'💧', 'STF-APH':'🔧',
      'KNPC MF':'🏗️', 'ALL':'🌐'
    };

    // ── Sites fallback: derived from masterEquipmentDb (used until API loads) ──
    function buildDefaultSites() {
        const siteMap = {};
        masterEquipmentDb.forEach(e => {
            if(e.site === 'ALL') return;
            if(!siteMap[e.site]) siteMap[e.site] = {id: e.site, icon: siteIcons[e.site]||'📍', elec:0, mech:0, inst:0};
            if(e.dept === 'Electrical') siteMap[e.site].elec++;
            else if(e.dept === 'Mechanical') siteMap[e.site].mech++;
            else if(e.dept === 'Instrument') siteMap[e.site].inst++;
        });
        const instGlobal = masterEquipmentDb.filter(e => e.site === 'ALL' && e.dept === 'Instrument').length;
        Object.values(siteMap).forEach(s => s.inst += instGlobal);
        return Object.values(siteMap).sort((a,b) => a.id.localeCompare(b.id));
    }
    const defaultSites = buildDefaultSites();

    // ─── SHARED STATE (populated from API) ───────────────────
    const DEFAULT_MOTHBALLED_EQUIP_DB = [
        {
            uid:'mb-001',
            assetName:'CRUDE EXPORT PUMP 612-G-23',
            equipmentTag:'612-G-23',
            site:'NPP',
            dept:'Mechanical',
            mothballedOn:'2026-01-14',
            mothballedBy:'Planner',
            futurePlan:'Keep preserved until export duty review is finalized; seal replacement and alignment check planned before reactivation.',
            reactivatedOn:'',
            reactivatedBy:''
        },
        {
            uid:'mb-002',
            assetName:'STANDBY GEN – COCC Lab',
            equipmentTag:'621-SDG-COCC-LAB',
            site:'COCC',
            dept:'Electrical',
            mothballedOn:'2025-11-09',
            mothballedBy:'EO',
            futurePlan:'Retained as strategic spare pending smart power distribution upgrade and cable rerouting decision.',
            reactivatedOn:'',
            reactivatedBy:''
        },
        {
            uid:'mb-003',
            assetName:'Flow Computer (Metering)',
            equipmentTag:'FLOW-COMP',
            site:'ALL',
            dept:'Instrument',
            mothballedOn:'2025-12-03',
            mothballedBy:'Planner',
            futurePlan:'Awaiting final integration scope with unified dashboard project before reinstatement or replacement.',
            reactivatedOn:'',
            reactivatedBy:''
        },
        {
            uid:'mb-004',
            assetName:'PURGE AIR COMPRESSOR B',
            equipmentTag:'612-K-28B',
            site:'NPP',
            dept:'Mechanical',
            mothballedOn:'2026-02-21',
            mothballedBy:'EO',
            futurePlan:'Under preservation. Decision pending whether to overhaul existing unit or retain as donor machine for spares.',
            reactivatedOn:'',
            reactivatedBy:''
        },
        {
            uid:'mb-005',
            assetName:'Loading Status – CPH',
            equipmentTag:'LOAD-CPH',
            site:'MINA',
            dept:'Instrument',
            mothballedOn:'2025-10-18',
            mothballedBy:'Planner',
            futurePlan:'Panel retained in standby while replacement communication architecture is reviewed with ETS.',
            reactivatedOn:'',
            reactivatedBy:''
        },
        {
            uid:'mb-006',
            assetName:'EMERG. GENERATOR – STF A32A',
            equipmentTag:'279-EDG-092',
            site:'STF',
            dept:'Electrical',
            mothballedOn:'2025-08-27',
            mothballedBy:'Planner',
            futurePlan:'Returned to service after preservation closeout and load test completion.',
            reactivatedOn:'2026-03-11',
            reactivatedBy:'Electrical'
        },
        {
            uid:'mb-007',
            assetName:'EFFLUENT WATER PUMP B',
            equipmentTag:'620-G-601B',
            site:'NTF',
            dept:'Mechanical',
            mothballedOn:'2025-09-06',
            mothballedBy:'EO',
            futurePlan:'Reactivated after standby line flushing and seal-box inspection.',
            reactivatedOn:'2026-02-04',
            reactivatedBy:'Mechanical'
        },
        {
            uid:'mb-008',
            assetName:'Fire Alarm Panel',
            equipmentTag:'FAP',
            site:'ALL',
            dept:'Instrument',
            mothballedOn:'2025-07-19',
            mothballedBy:'Planner',
            futurePlan:'Old panel preserved as contingency backup until project handover was accepted.',
            reactivatedOn:'2026-01-16',
            reactivatedBy:'Instrument'
        }
    ];

    let sitesDb       = [...defaultSites];
    let issuesDb      = [];
    let tankMaintDb   = [];
    let mothballedEquipDb = DEFAULT_MOTHBALLED_EQUIP_DB.map(item => ({ ...item }));
    let userEquipDb   = [];
    let shortStatusDb = [];
    let auditLog      = [];
    let currentUser   = null;
    let activeOpsPage = 'equipment';

    // ─── PERMISSION DEFINITIONS ──────────────────────────────
    const PERM_GROUPS = [
        { short:'Equip.Status', group:'📊 Equipment Status', perms:[
            { id:'post',       label:'Post & Edit Issues' },
            { id:'tank',       label:'Tank on Maintenance' },
            { id:'mothballed', label:'Mothballed Equipment' },
            { id:'equipment',  label:'Equipment Master List' },
            { id:'status',     label:'Short Statuses' },
            { id:'sites',      label:'Sites Management' },
        ]},
        { short:'EO', group:'🛢️ EO Dashboard', perms:[
            { id:'eot.view',    label:'View EO Dashboard' },
            { id:'eot.edit',    label:'Edit EO Records' },
            { id:'eot.approve', label:'Approve (TLEO)' },
        ]},
        { short:'ETS', group:'⚗️ ETS Dashboard', perms:[
            { id:'ets.view',    label:'View ETS Dashboard' },
            { id:'ets.edit',    label:'Edit ETS Records' },
            { id:'ets.approve', label:'Approve (TLETS)' },
        ]},
        { short:'EOM&M', group:'🔩 EOM&M Dashboard', perms:[
            { id:'eomm.view',    label:'View EOM&M Dashboard' },
            { id:'eomm.edit',    label:'Edit Records' },
            { id:'eomm.approve', label:'Approve (TLEOMM)' },
        ]},
        { short:'HSE', group:'🦺 HSE Dashboard', perms:[
            { id:'hse.view',    label:'View HSE Dashboard' },
            { id:'hse.edit',    label:'Edit Records' },
            { id:'hse.approve', label:'Approve (TLHSE)' },
        ]},
        { short:'Team Mtg', group:'⚙️ EMT Team Meeting', perms:[
            { id:'team.view',    label:'View Meeting Board' },
            { id:'team.edit',    label:'Edit Actions' },
            { id:'team.approve', label:'TL Approve' },
        ]},
        { short:'MOM', group:'📋 Group Meeting MOM', perms:[
            { id:'mom.contribute', label:'Contribute & Comment' },
            { id:'mom.approve',    label:'Approve (TL Section)' },
            { id:'mom.publish',    label:'Publish MOM (Planner)' },
            { id:'mom.final',      label:'Final Approval (Manager)' },
        ]},
        { short:'Progress', group:'📈 Monthly Progress', perms:[
            { id:'progress.edit',    label:'Edit during Draft' },
            { id:'progress.endorse', label:'Senior Engineer Endorse' },
            { id:'progress.approve', label:'TL Approval' },
            { id:'progress.publish', label:'Publish (Planner)' },
            { id:'progress.final',   label:'Final Approval (Manager)' },
        ]},
        { short:'Budget', group:'💰 Budget Management', perms:[
            { id:'budget.log',     label:'Log Expenditure' },
            { id:'budget.view',    label:'Full Budget View' },
            { id:'budget.approve', label:'Approve Accounts' },
        ]},
        { short:'Memo', group:'📨 Internal Memo', perms:[
            { id:'memo.view',    label:'View & Track Memos' },
            { id:'memo.approve', label:'Approve Memos' },
        ]},
    ];

    const ROLE_DEFAULTS = {
        'TLEMT':               ['post','tank','equipment','status','sites','team.view','team.edit','team.approve','mom.contribute','mom.approve','progress.edit','progress.approve','budget.view','budget.approve','memo.view','memo.approve'],
        'Electrical':          ['post','equipment','team.view','team.edit','mom.contribute','progress.edit'],
        'Mechanical':          ['post','equipment','team.view','team.edit','mom.contribute','progress.edit'],
        'Instrument':          ['post','equipment','team.view','team.edit','mom.contribute','progress.edit'],
        'Building Maintenance':['post','equipment','team.view','team.edit','mom.contribute'],
        'MEO':                 ['eot.view','ets.view','eomm.view','hse.view','team.view','mom.contribute','mom.final','progress.edit','progress.final','budget.view','memo.view'],
        'Planner':             ['post','tank','equipment','status','sites','team.view','team.edit','mom.contribute','mom.publish','progress.edit','progress.publish','budget.view','memo.view'],
        'EO':                  ['tank','mothballed','eot.view','eot.edit','mom.contribute'],
        'EOT':                 ['eot.view','eot.edit','mom.contribute','progress.edit'],
        'ETS':                 ['ets.view','ets.edit','mom.contribute','progress.edit'],
        'OilMov':              ['mom.contribute'],
        'HSE':                 ['hse.view','hse.edit','mom.contribute','progress.edit'],
    };

    function permElId(id) { return 'perm-' + id.replace(/\./g, '-'); }

    function buildPermMatrixUI() {
        const wrap = document.getElementById('perm-matrix-wrap');
        if (!wrap) return;
        const pageRefs = {
            'Equip.Status':'emt.html (Equipment Status)',
            'EO':'EOT.html',
            'ETS':'ETS.html',
            'EOM&M':'eomm.html',
            'HSE':'hse.html',
            'Team Mtg':'team.html',
            'MOM':'mom.html',
            'Progress':'progress.html',
            'Budget':'budget.html',
            'Memo':'memo.html'
        };
        wrap.innerHTML = PERM_GROUPS.map(g => `
            <div class="perm-group">
                <div class="perm-group-hdr">${g.group} <span style="font-size:10px;color:var(--text-muted);font-weight:400;">[${pageRefs[g.short]||g.short}]</span></div>
                <div class="perm-items">
                    ${g.perms.map(p => `<label class="perm-item"><input type="checkbox" id="${permElId(p.id)}"> ${p.label}</label>`).join('')}
                </div>
            </div>`).join('');
    }

    function applyRoleDefaults() {
        const role = document.getElementById('admin-role').value;
        const defaults = ROLE_DEFAULTS[role] || [];
        PERM_GROUPS.forEach(g => g.perms.forEach(p => {
            const el = document.getElementById(permElId(p.id));
            if (el) el.checked = defaults.includes(p.id);
        }));
    }

    function collectPermissions() {
        return PERM_GROUPS.flatMap(g => g.perms.map(p => p.id))
            .filter(id => { const el = document.getElementById(permElId(id)); return el && el.checked; });
    }

    // ─── BACKEND AUTO-DETECT ─────────────────────────────────
    // Prefer the likely backend for the current host, but always probe both files.
    // This avoids silent failures when the page is opened from a different host binding.
    // Clear any previously cached backend so stale 'EMT_Handler.ashx' doesn't persist
    try{ const cb=sessionStorage.getItem('emt_backend'); if(cb==='EMT_Handler.ashx') sessionStorage.removeItem('emt_backend'); }catch(e){}
    let BACKEND = (() => { try{ const b=sessionStorage.getItem('emt_backend'); return b||''; }catch(e){ return ''; } })();
    const BACKEND_CANDIDATES = (() => {
        const forced = new URLSearchParams(location.search).get('backend');
        if (forced === 'ashx') return ['EMT_Handler.ashx', 'api.php'];
        return ['EMT_Handler.ashx', 'api.php'];
    })();

    async function ensureBackend() {
        if (BACKEND) return BACKEND;
        for (const candidate of BACKEND_CANDIDATES) {
            try {
                const res = await fetch(candidate + '?action=me', {
                    method: 'GET',
                    credentials: 'same-origin',
                    headers: { 'Accept': 'application/json' }
                });
                // Must be JSON (not raw .ashx source served by Apache)
                const ct = res.headers.get('Content-Type') || '';
                if ((res.ok || res.status === 401) && ct.includes('json')) {
                    BACKEND = candidate;
                    try{sessionStorage.setItem('emt_backend',BACKEND);}catch(e){}
                    return BACKEND;
                }
            } catch (err) {
                console.warn('Backend probe failed:', candidate, err);
            }
        }
        BACKEND = BACKEND_CANDIDATES[0];
        try{sessionStorage.setItem('emt_backend',BACKEND);}catch(e){}
        return BACKEND;
    }

    function apiUrl(path) {
        path = path.replace(/^\//, '').replace(/^api\//, '');
        const parts = path.split('/');
        const action = parts[0];
        let qs = BACKEND + '?action=' + action;

        if (action === 'sites'     && parts[1]) qs += '&id='       + encodeURIComponent(parts[1]);
        if (action === 'issues'    && parts[1]) qs += '&uid='      + encodeURIComponent(parts[1]);
        if (action === 'tank_maintenance' && parts[1]) qs += '&uid=' + encodeURIComponent(parts[1]);
        if (action === 'mothballed_equipment' && parts[1]) qs += '&uid=' + encodeURIComponent(parts[1]);
        if (action === 'statuses'  && parts[1]) qs += '&id='       + encodeURIComponent(parts[1]);
        if (action === 'equipment' && parts[1]) qs += '&tag='      + encodeURIComponent(parts[1])
                                                   + '&site='     + encodeURIComponent(parts[2]||'');
        if (action === 'users'     && parts[1]) qs += '&username=' + encodeURIComponent(parts[1])
                                                   + (parts[2] ? '&type=' + parts[2] : '');
        if (action === 'mom') {
            qs = BACKEND + '?action=mom_' + parts[1];
            if (parts[2]) qs += '&id=' + encodeURIComponent(parts[2]);
        }
        return qs;
    }

    async function api(method, path, body) {
        await ensureBackend();
        let url = apiUrl(path);
        let fetchMethod = method;
        // IIS blocks PUT/DELETE in many configurations (WebDAV, requestFiltering).
        // Send them as POST with ?_method=PUT/DELETE — the server reads the override.
        if (method === 'PUT' || method === 'DELETE') {
            url += '&_method=' + method;
            fetchMethod = 'POST';
        }
        const opts = {
            method: fetchMethod,
            headers: {
                'Accept': 'application/json',
                'Content-Type': 'application/json; charset=UTF-8'
            },
            credentials:'same-origin'
        };
        if (body) opts.body = JSON.stringify(body);
        const res  = await fetch(url, opts);
        if (res.status === 401) { currentUser = null; showAuthModal(); throw new Error('Not logged in'); }
        const data = await res.json().catch(() => ({}));
        if (!res.ok) throw new Error(data.error || 'Server error');
        return data;
    }

    let backendDataLoaded = false;

    async function loadDashboardData() {
        try {
            const [sites, issues, tanks, mothballed] = await Promise.all([
                api('GET', '/sites'),
                api('GET', '/issues'),
                api('GET', '/tank_maintenance'),
                api('GET', '/mothballed_equipment'),
            ]);
            sitesDb       = sites;
            issuesDb      = issues;
            tankMaintDb   = tanks;
            mothballedEquipDb = mothballed;
        } catch(e) {
            console.error('loadDashboardData error:', e);
        }
    }

    async function loadBackendData(force = false) {
        if (backendDataLoaded && !force) return;
        try {
            const [equip, statuses] = await Promise.all([
                api('GET', '/equipment'),
                api('GET', '/statuses'),
            ]);
            userEquipDb   = equip;
            shortStatusDb = statuses;
            backendDataLoaded = true;
        } catch(e) {
            console.error('loadBackendData error:', e);
        }
    }

    // saveData is now a no-op (data saved via individual API calls)
    function saveData() {}

    // logAction is now a no-op (server logs automatically on every mutating API call)
    function logAction() {}

    // ─── TAB SWITCHING ───────────────────────────────────────
    const ALL_TABS = ['issues','tankmaint','mothballed','sites','equip','status','history','admin'];
    function isPortalAdmin() {
        return currentUser && currentUser.role === 'Admin';
    }
    function canAccessBackendPortal(user = currentUser) {
        if (!user) return false;
        if (['Admin', 'Planner', 'TLEMT', 'EO', 'MEO'].includes(user.role)) return true;
        const perms = user.permissions || [];
        return perms.some(p => ['post','tank','mothballed','equipment','status','sites'].includes(p));
    }
    function canManageTankMaintenance(user = currentUser) {
        if (!user) return false;
        return ['Admin', 'EO'].includes(user.role) || (user.permissions || []).includes('tank');
    }
    function canManageMothballedEquipment(user = currentUser) {
        if (!user) return false;
        return ['Admin', 'EO'].includes(user.role) || (user.permissions || []).includes('mothballed');
    }
    function showPortalAccessMessage(tab) {
        const messages = {
            admin: 'This section is restricted. You can continue using Equipment Status and the pages available for your login.',
            history: 'This section is restricted. You can continue using Equipment Status and the pages available for your login.',
            tankmaint: 'Tank on Maintenance records can only be managed by EO members. You can continue viewing Equipment Status and other pages available for your login.',
            mothballed: 'Mothballed equipment records can only be managed by EO members. You can continue viewing Equipment Status and other pages available for your login.',
            issues: 'Posting issues is authorized for TLEMT and Planner roles. You can continue using the tabs available for your login.',
            sites: 'Site management is authorized for TLEMT and Planner roles. You can continue using the tabs available for your login.'
        };
        alert(messages[tab] || 'This section is not available for your login. You can continue using Equipment Status and the pages available for your login.');
    }
    function showTankMaintenanceAccessMessage() {
        showPortalAccessMessage('tankmaint');
    }
    function showMothballedAccessMessage() {
        showPortalAccessMessage('mothballed');
    }
    function openTankMaintenanceManager() {
        if (!currentUser) {
            showAuthModal();
            return;
        }
        if (!canManageTankMaintenance()) {
            showTankMaintenanceAccessMessage();
            return;
        }
        openBackend();
        setTimeout(() => switchTab('tankmaint'), 80);
    }
    function openMothballedManager() {
        if (!currentUser) {
            showAuthModal();
            return;
        }
        if (!canManageMothballedEquipment()) {
            showMothballedAccessMessage();
            return;
        }
        openBackend();
        setTimeout(() => switchTab('mothballed'), 80);
    }
    function switchTab(tab) {
        const isEO = currentUser && currentUser.role === 'EO';
        const fallback = isEO ? 'tankmaint' : 'issues';
        if (tab === 'admin' && !isPortalAdmin()) {
            showPortalAccessMessage('admin');
            tab = fallback;
        }
        if (tab === 'tankmaint' && !canManageTankMaintenance()) {
            showTankMaintenanceAccessMessage();
            tab = fallback;
        }
        if (tab === 'history' && !isPortalAdmin()) {
            showPortalAccessMessage('history');
            tab = fallback;
        }
        if (tab === 'mothballed' && !canManageMothballedEquipment()) {
            showMothballedAccessMessage();
            tab = fallback;
        }
        if ((tab === 'issues' || tab === 'sites') && isEO) {
            showPortalAccessMessage(tab);
            tab = 'tankmaint';
        }
        ALL_TABS.forEach(t => {
            const tabBtn = document.getElementById('ptab-'+t);
            const panel = document.getElementById('panel-'+t);
            if (tabBtn) tabBtn.classList.toggle('active', t===tab);
            if (panel) panel.style.display = t===tab ? 'block' : 'none';
        });
        if(tab==='history') loadAuditLog();
        if(tab==='equip')   { populateEqSiteFilter(); renderEquipTable(); }
        if(tab==='status')  renderStatusTable();
        if(tab==='sites')   renderAdminSitesTable();
        if(tab==='tankmaint') renderTankBackendTable();
        if(tab==='mothballed') renderMothballedBackendTable();
    }

    function allEquipment() { return [...masterEquipmentDb, ...userEquipDb]; }
    function getEquipForRole(role, siteFilter) {
        let list = allEquipment();
        if(role && role !== 'Admin' && role !== 'TLEMT') list = list.filter(e => e.dept === role);
        if(siteFilter && siteFilter !== 'ALL') list = list.filter(e => e.site === siteFilter || e.site === 'ALL');
        return list;
    }

    // ─── LOGIN WALL ──────────────────────────────────────────
    function showLoginWall() { document.getElementById('login-wall').style.display = 'flex'; }
    function showProtectedContent() { document.getElementById('login-wall').style.display = 'none'; }

    // ─── AUTH ────────────────────────────────────────────────
    function showAuthModal()  { document.getElementById('auth-modal').style.display = 'flex'; }
    function closeAuthModal() {
        document.getElementById('auth-modal').style.display = 'none';
        document.getElementById('auth-error').style.display = 'none';
    }

    async function processLogin() {
        const username = document.getElementById('auth-user').value.trim();
        const password = document.getElementById('auth-pass').value.trim();
        try {
            const user = await api('POST', '/login', { username, password });
            currentUser = user;
            try{sessionStorage.setItem('emt_s',JSON.stringify(user));}catch(e){}
            closeAuthModal();
            document.getElementById('auth-user').value = '';
            document.getElementById('auth-pass').value = '';
            // Update login button to show user
            document.getElementById('nav-login-btn').innerHTML =
                `<svg width="13" height="13" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="8" r="4"/><path d="M4 20c0-4 3.6-7 8-7s8 3 8 7"/></svg>
                ${user.username} (${user.role})`;
            if (canAccessBackendPortal(user)) document.getElementById('nav-login-btn').onclick = openBackend;
            // Update topbar
            document.getElementById('topbar-login-btn').style.display = 'none';
            document.getElementById('topbar-user-label').textContent = user.username + ' (' + displayRoleLabel(user.role) + ')';
            document.getElementById('topbar-user-pill').style.display = 'flex';
            const canEntry = canAccessBackendPortal(user);
            document.getElementById('btn-tb-entry').style.display  = canEntry ? '' : 'none';
            document.getElementById('btn-tb-export').style.display = '';
            const navAdminLogin = document.getElementById('nav-admin');
            if(navAdminLogin) navAdminLogin.style.display = (user.role==='Admin') ? 'flex' : 'none';
            location.reload();
            return;
        } catch(e) {
            document.getElementById('auth-error').style.display = 'block';
            document.getElementById('auth-error').innerText = e.message || 'Invalid credentials';
        }
    }

    async function logout() {
        await api('POST', '/logout').catch(()=>{});
        currentUser = null;
        try{sessionStorage.removeItem('emt_s');}catch(e){}
        document.getElementById('backend-view').style.display   = 'none';
        document.getElementById('dashboard-view').style.display = 'block';
        document.getElementById('nav-login-btn').style.display  = 'flex';
        document.getElementById('nav-login-btn').innerHTML = `<svg width="13" height="13" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"><rect x="3" y="11" width="18" height="11" rx="2"/><path d="M7 11V7a5 5 0 0 1 10 0v4"/></svg> Login`;
        document.getElementById('nav-login-btn').onclick = showAuthModal;
        // Reset topbar
        document.getElementById('topbar-user-pill').style.display = 'none';
        document.getElementById('topbar-login-btn').style.display = '';
        const navAdminOut = document.getElementById('nav-admin');
        if(navAdminOut) navAdminOut.style.display = 'none';
        document.getElementById('btn-tb-entry').style.display  = 'none';
        document.getElementById('btn-tb-export').style.display = 'none';
        showLoginWall();
    }

    // ─── BACKEND OPEN ────────────────────────────────────────
    async function openBackend() {
        document.getElementById('dashboard-view').style.display = 'none';
        if (document.getElementById('nav-login-btn')) document.getElementById('nav-login-btn').style.display = 'none';
        document.getElementById('backend-view').style.display   = 'block';
        const naOpen = document.getElementById('nav-admin'); if(naOpen) naOpen.style.display = 'none';
        // Topbar Data Entry btn becomes "← Dashboard"
        const btn = document.getElementById('btn-tb-entry');
        if (btn) { btn.textContent = '← Dashboard'; btn.onclick = closeBackend; }
        document.getElementById('current-username').innerText   = currentUser.username;
        document.getElementById('current-role').innerText       = currentUser.role;

        const isAdmin = currentUser.role === 'Admin' || currentUser.role === 'TLEMT';
        const isAdminOnly = currentUser.role === 'Admin';
        const canTank = canManageTankMaintenance();
        const canMothballed = canManageMothballedEquipment();
        const perms   = currentUser.permissions || [];
        const isEO = currentUser.role === 'EO';
        document.getElementById('ptab-history').style.display  = isAdmin ? '' : 'none';
        document.getElementById('ptab-admin').style.display    = 'none'; // Admin panel moved to admin.html
        document.getElementById('ptab-equip').style.display    = (!isEO && (isAdmin || perms.includes('equipment')))  ? '' : 'none';
        document.getElementById('ptab-status').style.display   = (!isEO && (isAdmin || perms.includes('status')))     ? '' : 'none';
        document.getElementById('ptab-tankmaint').style.display = canTank ? '' : 'none';
        document.getElementById('ptab-mothballed').style.display = canMothballed ? '' : 'none';
        // Tab visibility — EO only sees Tank+Mothballed; others controlled by permissions
        document.getElementById('ptab-issues').style.display   = (!isEO && (isAdmin || perms.includes('post')))  ? '' : 'none';
        document.getElementById('ptab-sites').style.display    = (!isEO && (isAdmin || perms.includes('sites'))) ? '' : 'none';
        // If user has no post permission, disable the submit button
        const submitBtn = document.querySelector('#panel-issues .btn-primary[onclick="submitNewIssue()"]');
        if(submitBtn) submitBtn.style.display = (isAdmin || perms.includes('post')) ? '' : 'none';

        if(isAdminOnly) {
            await loadAdminUsers();
        }
        const deptSel = document.getElementById('entry-dept');
        deptSel.disabled = !isAdmin;
        deptSel.value    = isAdmin ? '' : currentUser.role;

        const eqDeptSel = document.getElementById('eq-dept');
        eqDeptSel.disabled = !isAdmin;
        if (!isAdmin) eqDeptSel.value = currentUser.role;

        await loadBackendData();
        switchTab(isEO ? (canTank ? 'tankmaint' : 'mothballed') : (canTank && !isAdmin && !perms.includes('post') ? 'tankmaint' : (canMothballed && !isAdmin && !perms.includes('post') ? 'mothballed' : 'issues')));
        updateEntrySiteDropdown(); populateEqSiteFilter(); renderShortStatusDropdown();
        renderBackendTable(); renderAdminSitesTable(); renderTankBackendTable(); renderMothballedBackendTable();
    }

    function closeBackend() {
        document.getElementById('backend-view').style.display   = 'none';
        document.getElementById('dashboard-view').style.display = 'block';
        const btn = document.getElementById('btn-tb-entry');
        if (btn) { btn.innerHTML = '&#9881; Data Entry'; btn.onclick = openBackend; }
        const naClose = document.getElementById('nav-admin');
        if(naClose) naClose.style.display = (currentUser && currentUser.role==='Admin') ? 'flex' : 'none';
    }

    // ─── ADMIN: PASSWORDS ────────────────────────────────────
    async function adminChangePassword() {
        const username = document.getElementById('admin-user-select').value;
        const password = document.getElementById('admin-chg-pass').value.trim();
        if(!password) return;
        try {
            await api('PUT', `/users/${username}/password`, { password });
            document.getElementById('admin-pass-msg').innerText = 'Password updated!';
            document.getElementById('admin-chg-pass').value = '';
            setTimeout(() => document.getElementById('admin-pass-msg').innerText = '', 3000);
        } catch(e) { alert(e.message); }
    }

    // ─── USER MANAGEMENT ─────────────────────────────────────
    let adminUsersDb = [];

    async function loadAdminUsers() {
        adminUsersDb = await api('GET', '/users').catch(()=>[]);
        renderAdminUsers();
        // Populate password-change dropdown (non-admin users)
        const sel = document.getElementById('admin-user-select');
        if(sel) sel.innerHTML = adminUsersDb.filter(u=>u.role!=='Admin')
            .map(u=>`<option value="${u.username}">${u.username} (${u.role})</option>`).join('');
    }

    function displayRoleLabel(role) {
        if (role === 'Building Maintenance') return 'BM';
        if (role === 'TLEMT') return 'TL EMT';
        if (role === 'Manager' || role === 'MEO') return 'MEO';
        return role;
    }

    function renderAdminUsers() {
        const tbody = document.getElementById('admin-users-table'); if(!tbody) return;
        if(!adminUsersDb.length){ tbody.innerHTML=`<tr><td colspan="4" style="text-align:center;padding:16px;color:var(--text-muted);">No users found.</td></tr>`; return; }
        const totalPerms = PERM_GROUPS.reduce((a, g) => a + g.perms.length, 0);
        tbody.innerHTML = adminUsersDb.map(u => {
            const isSelf = u.username === currentUser?.username;
            const perms  = u.permissions || [];
            const defaults = ROLE_DEFAULTS[u.role] || [];
            const missingDefaults = defaults.filter(d => !perms.includes(d));
            const hasMismatch = u.role !== 'Admin' && missingDefaults.length > 0;
            const groupBadges = PERM_GROUPS.map(g => {
                const n = g.perms.filter(p => perms.includes(p.id)).length;
                if (!n) return '';
                const full = n === g.perms.length;
                return `<span style="font-size:10px;padding:2px 6px;border-radius:10px;margin:0 2px 2px 0;display:inline-block;font-weight:600;${full?'background:var(--good-bg);color:var(--good);':'background:var(--warn-bg);color:var(--warn);'}" title="${n}/${g.perms.length} privileges in ${g.group}">${g.short}</span>`;
            }).join('');
            const mismatchWarn = hasMismatch
                ? `<div style="font-size:10px;color:var(--crit);margin-top:3px;">⚠ Missing ${missingDefaults.length} default permission(s) for ${u.role} role</div>`
                : '';
            const syncBtn = hasMismatch && !isSelf
                ? `<br><button style="margin-top:4px;font-size:10px;padding:2px 7px;background:var(--warn-bg);color:var(--warn);border:1px solid var(--warn);border-radius:4px;cursor:pointer;font-weight:600;" onclick="adminSyncDefaults('${u.username}')">↺ Sync Defaults</button>`
                : '';
            const actions = isSelf
                ? `<span style="font-size:10px;color:var(--text-muted);">Current session</span>`
                : `<button class="btn-primary" style="padding:3px 8px;font-size:11px;width:auto;margin:0 3px 0 0;" onclick="adminEditUser('${u.username}')">Edit</button>`
                + (u.role!=='Admin' ? `<button class="btn-danger" onclick="adminDeleteUser('${u.username}')">Del</button>` : '')
                + syncBtn;
            return `<tr>
                <td style="font-weight:600;">${u.username}</td>
                <td><span class="role-badge">${displayRoleLabel(u.role)}</span></td>
                <td>
                    <div style="line-height:1.8;">${groupBadges||'<span style="font-size:11px;color:var(--text-muted);">No privileges</span>'}</div>
                    <div style="font-size:10px;color:var(--text-muted);margin-top:2px;">${perms.length} of ${totalPerms} privileges granted</div>
                    ${mismatchWarn}
                </td>
                <td style="white-space:nowrap;">${actions}</td>
            </tr>`;
        }).join('');
    }

    async function adminSaveUser() {
        const editUsername = document.getElementById('admin-edit-user').value;
        const username  = document.getElementById('admin-username').value.trim();
        const password  = document.getElementById('admin-new-pass').value.trim();
        const role      = document.getElementById('admin-role').value;
        const permissions = collectPermissions();
        const msgEl = document.getElementById('admin-msg');

        if(editUsername) {
            // Editing existing user
            try {
                await api('PUT', `/users/${editUsername}/permissions`, { role, permissions });
                if(password) await api('PUT', `/users/${editUsername}/password`, { password });
                msgEl.style.color='var(--good)'; msgEl.innerText='User updated!';
                setTimeout(()=>msgEl.innerText='',3000);
                await loadAdminUsers(); adminCancelEdit();
            } catch(e) { msgEl.style.color='var(--crit)'; msgEl.innerText=e.message; }
        } else {
            // Creating new user
            if(!username) { msgEl.style.color='var(--crit)'; msgEl.innerText='Username is required.'; return; }
            if(!password) { msgEl.style.color='var(--crit)'; msgEl.innerText='Password is required for new users.'; return; }
            try {
                await api('POST', '/users', { username, password, role, permissions });
                msgEl.style.color='var(--good)'; msgEl.innerText=`User "${username}" created!`;
                setTimeout(()=>msgEl.innerText='',3000);
                document.getElementById('admin-username').value='';
                document.getElementById('admin-new-pass').value='';
                await loadAdminUsers();
            } catch(e) { msgEl.style.color='var(--crit)'; msgEl.innerText=e.message; }
        }
    }

    async function adminSyncDefaults(username) {
        const u = adminUsersDb.find(x => x.username === username);
        if (!u) return;
        const defaults = ROLE_DEFAULTS[u.role] || [];
        if (!defaults.length) { alert('No role defaults defined for role: ' + u.role); return; }
        if (!confirm('Reset permissions for "' + username + '" to the defaults for ' + u.role + ' role?\n\nWill set: ' + defaults.join(', '))) return;
        const msgEl = document.getElementById('admin-msg');
        try {
            await api('PUT', `/users/${username}/permissions`, { role: u.role, permissions: defaults });
            msgEl.style.color='var(--good)'; msgEl.innerText='Permissions synced for ' + username + '!';
            setTimeout(()=>msgEl.innerText='',3000);
            await loadAdminUsers();
        } catch(e) { msgEl.style.color='var(--crit)'; msgEl.innerText=e.message; }
    }

    function adminEditUser(username) {
        const u = adminUsersDb.find(x=>x.username===username); if(!u) return;
        document.getElementById('admin-form-title').innerText = `Edit User — ${username}`;
        document.getElementById('admin-edit-user').value  = username;
        document.getElementById('admin-username').value   = username;
        document.getElementById('admin-username').readOnly= true;
        document.getElementById('admin-new-pass').value   = '';
        document.getElementById('admin-role').value       = u.role;
        document.getElementById('admin-pass-hint').innerText = '(leave blank to keep current)';
        PERM_GROUPS.forEach(g => g.perms.forEach(p => {
            const el = document.getElementById(permElId(p.id));
            if (el) el.checked = u.permissions && u.permissions.includes(p.id);
        }));
        document.getElementById('admin-cancel-edit').style.display='';
        const btn = document.querySelector('#panel-admin .btn-primary[onclick="adminSaveUser()"]');
        if(btn) btn.innerText='Save Changes';
        document.getElementById('admin-form-title').scrollIntoView({behavior:'smooth',block:'nearest'});
    }

    function adminCancelEdit() {
        document.getElementById('admin-form-title').innerText = 'Create New User';
        document.getElementById('admin-edit-user').value  = '';
        document.getElementById('admin-username').value   = '';
        document.getElementById('admin-username').readOnly= false;
        document.getElementById('admin-new-pass').value   = '';
        document.getElementById('admin-pass-hint').innerText='(required for new user)';
        document.getElementById('admin-role').value       = 'Electrical';
        applyRoleDefaults();
        document.getElementById('admin-cancel-edit').style.display='none';
        const btn = document.querySelector('#panel-admin .btn-primary[onclick="adminSaveUser()"]');
        if(btn) btn.innerText='Create User';
        document.getElementById('admin-msg').innerText='';
    }

    async function adminDeleteUser(username) {
        if(!confirm(`Delete user "${username}"? This cannot be undone.`)) return;
        try {
            await api('DELETE', `/users/${username}`);
            await loadAdminUsers();
        } catch(e) { alert(e.message); }
    }

    // ─── SITES ───────────────────────────────────────────────
    async function adminSaveSite() {
        const id   = document.getElementById('admin-site-id').value.trim().toUpperCase();
        const icon = document.getElementById('admin-site-icon').value.trim() || '📍';
        const elec = parseInt(document.getElementById('admin-site-elec').value) || 0;
        const mech = parseInt(document.getElementById('admin-site-mech').value) || 0;
        const inst = parseInt(document.getElementById('admin-site-inst').value) || 0;
        if(!id) return alert("Site ID is required.");
        try {
            await api('PUT', `/sites/${id}`, { icon, elec, mech, inst });
            sitesDb = await api('GET', '/sites');
            renderAdminSitesTable(); updateEntrySiteDropdown(); populateEqSiteFilter(); renderDashboard();
            document.getElementById('admin-site-id').value = '';
            ['admin-site-icon','admin-site-elec','admin-site-mech','admin-site-inst'].forEach((el,i)=>document.getElementById(el).value=i===0?'':'0');
        } catch(e) { alert(e.message); }
    }

    function adminEditSite(id) {
        const s = sitesDb.find(x=>x.id===id); if(!s) return;
        document.getElementById('admin-site-id').value   = s.id;
        document.getElementById('admin-site-icon').value = s.icon;
        document.getElementById('admin-site-elec').value = s.elec;
        document.getElementById('admin-site-mech').value = s.mech;
        document.getElementById('admin-site-inst').value = s.inst;
        switchTab('sites');
    }

    async function adminDeleteSite(id) {
        if(!confirm(`Delete site ${id}?`)) return;
        try {
            await api('DELETE', `/sites/${id}`);
            sitesDb = await api('GET', '/sites');
            renderAdminSitesTable(); updateEntrySiteDropdown(); renderDashboard();
        } catch(e) { alert(e.message); }
    }

    function renderAdminSitesTable() {
        const tbody = document.getElementById('admin-sites-table'); if(!tbody) return;
        tbody.innerHTML = sitesDb.map(s=>`<tr>
            <td><strong>${s.id}</strong></td><td>${s.icon}</td>
            <td>${s.elec}</td><td>${s.mech}</td><td>${s.inst}</td>
            <td><button class="btn-primary" style="padding:3px 8px;font-size:11px;width:auto;margin:0 4px 0 0;" onclick="adminEditSite('${s.id}')">Edit</button>
                <button class="btn-danger" onclick="adminDeleteSite('${s.id}')">Del</button></td>
        </tr>`).join('');
    }

    // ─── EQUIPMENT ───────────────────────────────────────────
    function updateEntrySiteDropdown() {
        const deptSel = document.getElementById('entry-dept');
        const role    = (currentUser && currentUser.role !== 'Admin' && currentUser.role !== 'TLEMT') ? currentUser.role : (deptSel ? deptSel.value : '');
        const sel     = document.getElementById('entry-site');
        let sites = [...new Set(sitesDb.map(s=>s.id))].sort();
        sel.innerHTML = '<option value="ALL">\u2014 All Sites \u2014</option>' + sites.map(s=>`<option value="${s}">${s}</option>`).join('');
        filterEquipBysite();
        renderShortStatusDropdown();
    }

    function filterEquipBysite() {
        const site    = document.getElementById('entry-site').value;
        const deptSel = document.getElementById('entry-dept');
        const role    = (currentUser && currentUser.role !== 'Admin' && currentUser.role !== 'TLEMT') ? currentUser.role : (deptSel ? deptSel.value : '');
        const equipSel= document.getElementById('entry-equip-select');
        const list    = getEquipForRole(role, site);
        const bysite  = {};
        list.forEach(e => { if(!bysite[e.site]) bysite[e.site]=[]; bysite[e.site].push(e); });
        let html = '<option value="">\u2014 Choose equipment \u2014</option>';
        Object.entries(bysite).sort().forEach(([grp,items]) => {
            html += `<optgroup label="${grp}">`;
            items.forEach(e => { html += `<option value="${grp}||${e.tag}||${e.name}||${e.dept}">[${e.tag}] ${e.name}</option>`; });
            html += '</optgroup>';
        });
        equipSel.innerHTML = html;
        document.getElementById('entry-id').value = '';
        document.getElementById('entry-name').value = '';
    }

    function autofillEquip() {
        const val = document.getElementById('entry-equip-select').value; if(!val) return;
        const [site,tag,name,dept] = val.split('||');
        document.getElementById('entry-id').value   = tag;
        document.getElementById('entry-name').value = name;
        const siteSel = document.getElementById('entry-site');
        for(let o of siteSel.options) { if(o.value===site){siteSel.value=site;break;} }
        if(currentUser && currentUser.role==='Admin') {
            const ds = document.getElementById('entry-dept'); if(!ds.disabled) ds.value = dept;
        }
    }

    function populateEqSiteFilter() {
        const sites = [...new Set(sitesDb.map(s=>s.id))].sort();
        const opts  = sites.map(s=>`<option value="${s}">${s}</option>`).join('');
        const qs = document.getElementById('eq-site');    if(qs) qs.innerHTML = opts;
        const qf = document.getElementById('eq-filter-site'); if(qf) qf.innerHTML = '<option value="">All Sites</option>'+opts;
    }

    function renderEquipTable() {
        const tbody = document.getElementById('equip-table-body'); if(!tbody) return;
        const search = (document.getElementById('eq-search')?.value||'').toLowerCase();
        const fdept  = document.getElementById('eq-filter-dept')?.value||'';
        const fsite  = document.getElementById('eq-filter-site')?.value||'';
        let list = allEquipment();
        if(search) list = list.filter(e=>e.name.toLowerCase().includes(search)||e.tag.toLowerCase().includes(search));
        if(fdept)  list = list.filter(e=>e.dept===fdept);
        if(fsite)  list = list.filter(e=>e.site===fsite);
        const cnt = document.getElementById('eq-count'); if(cnt) cnt.innerText=`(${list.length} items)`;
        const isAdminView = currentUser && (currentUser.role === 'Admin' || currentUser.role === 'TLEMT');
        tbody.innerHTML = list.map(e => {
            const isMaster = !e._user;
            const canEdit  = isAdminView || e.dept === currentUser?.role;
            const btns = canEdit
                ? `<button class="btn-primary" style="padding:3px 8px;font-size:11px;width:auto;margin:0 3px 0 0;" onclick="editEquipment('${e.tag}','${e.site}',${isMaster})">Edit</button>`
                  + (!isMaster ? `<button class="btn-danger" onclick="deleteEquipment('${e.tag}','${e.site}')">Del</button>` : '')
                : `<span style="font-size:10px;color:var(--text-muted);">—</span>`;
            const badge = isMaster
                ? `<span style="font-size:10px;padding:2px 6px;border-radius:10px;font-weight:600;background:var(--accent-light);color:var(--accent);">Master</span>`
                : `<span style="font-size:10px;padding:2px 6px;border-radius:10px;font-weight:600;background:var(--good-bg);color:var(--good);">Added</span>`;
            return `<tr><td style="font-family:'DM Mono',monospace;font-size:11px;">${e.tag}</td><td>${e.name}</td><td>${e.dept}</td><td>${e.site}</td><td>${badge}</td><td>${btns}</td></tr>`;
        }).join('') || `<tr><td colspan="6" style="text-align:center;padding:16px;color:var(--text-muted);">No equipment found</td></tr>`;
    }

    async function saveEquipment() {
        const name    = document.getElementById('eq-name').value.trim();
        const tag     = document.getElementById('eq-tag').value.trim();
        const dept    = document.getElementById('eq-dept').value;
        const site    = document.getElementById('eq-site').value;
        if(!name||!tag) return alert("Name and Tag are required.");
        try {
            await api('POST', '/equipment', { tag, name, dept, site });
            userEquipDb = await api('GET', '/equipment');
            renderEquipTable(); updateEntrySiteDropdown(); filterEquipBysite();
            ['eq-name','eq-tag','eq-edit-idx'].forEach(id=>document.getElementById(id).value='');
            document.getElementById('eq-cancel-btn').style.display='none';
        } catch(e) { alert(e.message); }
    }

    function editEquipment(tag, site, isMaster) {
        // Look in user-added first, then master list
        const e = userEquipDb.find(x=>x.tag===tag&&x.site===site)
               || masterEquipmentDb.find(x=>x.tag===tag&&x.site===site);
        if(!e) return;
        document.getElementById('eq-name').value     = e.name;
        document.getElementById('eq-tag').value      = e.tag;
        document.getElementById('eq-dept').value     = e.dept;
        document.getElementById('eq-site').value     = e.site;
        document.getElementById('eq-edit-idx').value = tag+'||'+site;
        document.getElementById('eq-cancel-btn').style.display = '';
        if(isMaster) {
            // Lock tag & dept for master entries (only name/site can change)
            document.getElementById('eq-tag').readOnly = true;
        } else {
            document.getElementById('eq-tag').readOnly = false;
        }
        document.getElementById('eq-name').closest('.entry-form-card').scrollIntoView({ behavior:'smooth', block:'nearest' });
    }

    function cancelEquipEdit() {
        ['eq-name','eq-tag','eq-edit-idx'].forEach(id=>document.getElementById(id).value='');
        document.getElementById('eq-tag').readOnly = false;
        document.getElementById('eq-cancel-btn').style.display='none';
    }

    async function deleteEquipment(tag, site) {
        if(!confirm(`Delete [${tag}]?`)) return;
        try {
            await api('DELETE', `/equipment/${encodeURIComponent(tag)}/${encodeURIComponent(site)}`);
            userEquipDb = await api('GET', '/equipment');
            renderEquipTable(); filterEquipBysite();
        } catch(e) { alert(e.message); }
    }

    // ─── SHORT STATUSES ──────────────────────────────────────
    function renderShortStatusDropdown() {
        const sel   = document.getElementById('entry-status'); if(!sel) return;
        const deptV = document.getElementById('entry-dept')?.value||'';
        const role  = (currentUser?.role==='Admin'||currentUser?.role==='TLEMT') ? deptV : (currentUser?.role||'');
        const list  = shortStatusDb.filter(s=>s.dept==='All'||!role||s.dept===role);
        sel.innerHTML = '<option value="">\u2014 Select status \u2014</option>' +
            list.map(s=>`<option value="${s.label}">${s.label}</option>`).join('');
    }

    function renderStatusTable() {
        const tbody = document.getElementById('status-table-body'); if(!tbody) return;
        tbody.innerHTML = shortStatusDb.map(s=>`<tr>
            <td style="font-weight:600;">${s.label}</td>
            <td><span style="font-size:10px;padding:2px 8px;border-radius:10px;font-weight:600;background:var(--accent-light);color:var(--accent);">${s.dept}</span></td>
            <td>
                <button class="btn-primary" style="padding:3px 8px;font-size:11px;width:auto;margin:0 3px 0 0;" onclick="editShortStatus(${s.id})">Edit</button>
                <button class="btn-danger" onclick="deleteShortStatus(${s.id})">Del</button>
            </td>
        </tr>`).join('') || `<tr><td colspan="3" style="text-align:center;padding:16px;color:var(--text-muted);">No statuses</td></tr>`;
    }

    async function saveShortStatus() {
        const label    = document.getElementById('ss-label').value.trim();
        const dept     = document.getElementById('ss-dept').value;
        const editId   = document.getElementById('ss-edit-idx').value;
        if(!label) return alert("Label is required.");
        try {
            if(editId) {
                await api('PUT', `/statuses/${editId}`, { label, dept });
            } else {
                await api('POST', '/statuses', { label, dept });
            }
            shortStatusDb = await api('GET', '/statuses');
            renderStatusTable(); renderShortStatusDropdown();
            document.getElementById('ss-label').value='';
            document.getElementById('ss-edit-idx').value='';
            document.getElementById('ss-cancel-btn').style.display='none';
        } catch(e) { alert(e.message); }
    }

    function editShortStatus(id) {
        const s = shortStatusDb.find(x=>x.id===id); if(!s) return;
        document.getElementById('ss-label').value    = s.label;
        document.getElementById('ss-dept').value     = s.dept;
        document.getElementById('ss-edit-idx').value = id;
        document.getElementById('ss-cancel-btn').style.display = '';
    }

    function cancelSSEdit() {
        document.getElementById('ss-label').value='';
        document.getElementById('ss-edit-idx').value='';
        document.getElementById('ss-cancel-btn').style.display='none';
    }

    async function deleteShortStatus(id) {
        const s = shortStatusDb.find(x=>x.id===id); if(!s) return;
        if(!confirm(`Delete "${s.label}"?`)) return;
        try {
            await api('DELETE', `/statuses/${id}`);
            shortStatusDb = await api('GET', '/statuses');
            renderStatusTable(); renderShortStatusDropdown();
        } catch(e) { alert(e.message); }
    }

    // ─── ISSUE SUBMIT / RESOLVE ──────────────────────────────
    async function submitNewIssue() {
        const dept   = document.getElementById('entry-dept').value.trim();
        const site   = document.getElementById('entry-site').value;
        const id     = document.getElementById('entry-id').value.trim();
        const name   = document.getElementById('entry-name').value.trim();
        const status = document.getElementById('entry-status').value;
        if(!dept||!id||!name){ alert("Department, Asset Tag and Equipment Name are required."); return; }
        const issue = {
            uid:      Date.now().toString(),
            id, name, dept,
            site:     site==='ALL'?'EXPORT':site,
            level:    document.getElementById('entry-level').value,
            status:   status||'Reported',
            update:   document.getElementById('entry-issue').value  ||'N/A',
            action:   document.getElementById('entry-action').value ||'Pending',
            remarks:  document.getElementById('entry-remarks').value||'\u2014',
            etr:      document.getElementById('entry-etr').value    ||'\u2014',
            date:     new Date().toISOString().split('T')[0],
            postedBy: currentUser.username
        };
        try {
            await api('POST', '/issues', issue);
            issuesDb = await api('GET', '/issues');
            renderBackendTable(); renderDashboard();
            ['entry-id','entry-name','entry-issue','entry-action','entry-remarks','entry-etr'].forEach(el=>document.getElementById(el).value='');
            document.getElementById('entry-equip-select').value='';
            document.getElementById('entry-status').value='';
            const btn = document.querySelector('#panel-issues .btn-primary[onclick="submitNewIssue()"]');
            if(btn){const orig=btn.innerText;btn.innerText='\u2713 Submitted!';btn.style.background='var(--good)';setTimeout(()=>{btn.innerText=orig;btn.style.background='';},2000);}
        } catch(e) { alert(e.message); }
    }

    async function deleteIssue(uid) {
        if(!confirm("Mark as resolved and remove this record?")) return;
        try {
            await api('DELETE', `/issues/${uid}`);
            issuesDb = await api('GET', '/issues');
            renderBackendTable(); renderDashboard();
        } catch(e) { alert(e.message); }
    }

    function editIssue(uid) {
        const i = issuesDb.find(x=>x.uid===uid); if(!i) return;
        // Load fields into the form
        document.getElementById('entry-dept').value  = i.dept;
        updateEntrySiteDropdown();
        document.getElementById('entry-site').value  = i.site;
        document.getElementById('entry-id').value    = i.id;
        document.getElementById('entry-name').value  = i.name;
        document.getElementById('entry-level').value = i.level;
        document.getElementById('entry-issue').value  = i.update  || '';
        document.getElementById('entry-action').value = i.action  || '';
        document.getElementById('entry-remarks').value= i.remarks || '';
        document.getElementById('entry-etr').value    = i.etr && i.etr!=='—' ? i.etr : '';
        renderShortStatusDropdown();
        document.getElementById('entry-status').value = i.status || '';
        // Switch form to edit mode
        document.getElementById('form-title').innerText = `Update Issue — ${i.id}`;
        const btn = document.querySelector('#panel-issues .btn-primary[onclick="submitNewIssue()"]');
        btn.innerText = 'Update Issue';
        btn.setAttribute('data-edit-uid', uid);
        btn.setAttribute('onclick', `updateIssue('${uid}')`);
        // Show cancel button
        let cancelBtn = document.getElementById('issue-cancel-edit-btn');
        if(!cancelBtn){
            cancelBtn = document.createElement('button');
            cancelBtn.id = 'issue-cancel-edit-btn';
            cancelBtn.className = 'btn-secondary';
            cancelBtn.style.marginTop = '6px';
            cancelBtn.innerText = 'Cancel Edit';
            cancelBtn.onclick = cancelIssueEdit;
            btn.parentNode.insertBefore(cancelBtn, btn.nextSibling);
        }
        cancelBtn.style.display = '';
        switchTab('issues');
        document.getElementById('form-title').scrollIntoView({ behavior:'smooth', block:'nearest' });
    }

    async function updateIssue(uid) {
        const issue = {
            level:   document.getElementById('entry-level').value,
            status:  document.getElementById('entry-status').value || 'Reported',
            update:  document.getElementById('entry-issue').value   || 'N/A',
            action:  document.getElementById('entry-action').value  || 'Pending',
            remarks: document.getElementById('entry-remarks').value || '—',
            etr:     document.getElementById('entry-etr').value     || '—',
        };
        try {
            await api('PUT', `/issues/${uid}`, issue);
            issuesDb = await api('GET', '/issues');
            renderBackendTable(); renderDashboard();
            cancelIssueEdit();
        } catch(e) { alert(e.message); }
    }

    function cancelIssueEdit() {
        document.getElementById('form-title').innerText = 'Post New Issue';
        const btn = document.querySelector('#panel-issues .btn-primary[onclick^="updateIssue"]')
                 || document.querySelector('#panel-issues .btn-primary[data-edit-uid]');
        if(btn){
            btn.innerText = 'Submit Issue to Dashboard';
            btn.removeAttribute('data-edit-uid');
            btn.setAttribute('onclick','submitNewIssue()');
        }
        const cancelBtn = document.getElementById('issue-cancel-edit-btn');
        if(cancelBtn) cancelBtn.style.display = 'none';
        ['entry-id','entry-name','entry-issue','entry-action','entry-remarks','entry-etr'].forEach(id=>{
            const el = document.getElementById(id); if(el) el.value='';
        });
        document.getElementById('entry-equip-select').value='';
        document.getElementById('entry-status').value='';
    }

    function renderBackendTable() {
        const tbody = document.getElementById('backend-table-body'); if(!tbody) return;
        const list  = (currentUser.role==='Admin'||currentUser.role==='TLEMT') ? issuesDb : issuesDb.filter(x=>x.dept===currentUser.role);
        if(!list.length){tbody.innerHTML=`<tr><td colspan="6" style="text-align:center;color:var(--good);padding:20px;font-weight:600;">\u2713 No active issues</td></tr>`;return;}
        tbody.innerHTML = list.map(i=>`<tr>
            <td><strong>${i.id}</strong><br><span style="font-size:11px;color:var(--text-muted)">${i.name}</span></td>
            <td>${i.site}</td><td>${i.dept}</td>
            <td class="${i.level==='crit'?'status-crit':'status-warn'}" style="font-weight:600;">${i.status}</td>
            <td style="font-size:11px;color:var(--text-muted);">${i.postedBy||'\u2014'}</td>
            <td style="white-space:nowrap;">
                <button class="btn-primary" style="padding:3px 8px;font-size:11px;width:auto;margin:0 3px 0 0;" onclick="editIssue('${i.uid}')">Edit</button>
                <button class="btn-danger" onclick="deleteIssue('${i.uid}')">Resolve</button>
            </td>
        </tr>`).join('');
    }

    // ─── TANK MAINTENANCE ───────────────────────────────────
    function fmtDate(value) {
        if (!value) return '—';
        const d = new Date(value + 'T00:00:00');
        if (Number.isNaN(d.getTime())) return value;
        return d.toLocaleDateString('en-GB', { day:'2-digit', month:'short', year:'numeric' });
    }

    function isTankActive(item) {
        return (item.status || 'Out of Commission') !== 'Returned';
    }

    function tankDaysToEdr(item) {
        if (!item.edr || !isTankActive(item)) return null;
        const today = new Date();
        today.setHours(0,0,0,0);
        const edr = new Date(item.edr + 'T00:00:00');
        if (Number.isNaN(edr.getTime())) return null;
        return Math.round((edr - today) / 86400000);
    }

    async function submitTankMaintenance() {
        const tankNo = document.getElementById('tank-no').value.trim();
        if (!tankNo) { alert('Tank number is required.'); return; }
        const payload = {
            uid: Date.now().toString(),
            tankNo,
            srNo: document.getElementById('tank-sr-no').value.trim(),
            handoverDate: document.getElementById('tank-handover-date').value || '',
            edr: document.getElementById('tank-edr').value || '',
            status: document.getElementById('tank-status').value || 'Out of Commission',
            actualReturnDate: document.getElementById('tank-return-date').value || '',
            remarks: document.getElementById('tank-remarks').value.trim()
        };
        try {
            await api('POST', '/tank_maintenance', payload);
            tankMaintDb = await api('GET', '/tank_maintenance');
            renderTankBackendTable();
            renderDashboard();
            cancelTankEdit();
        } catch(e) { alert(e.message); }
    }

    function editTankMaintenance(uid) {
        const item = tankMaintDb.find(x => x.uid === uid);
        if (!item) return;
        document.getElementById('tank-no').value = item.tankNo || '';
        document.getElementById('tank-sr-no').value = item.srNo || '';
        document.getElementById('tank-handover-date').value = item.handoverDate || '';
        document.getElementById('tank-edr').value = item.edr || '';
        document.getElementById('tank-status').value = item.status || 'Out of Commission';
        document.getElementById('tank-return-date').value = item.actualReturnDate || '';
        document.getElementById('tank-remarks').value = item.remarks || '';
        document.getElementById('tank-form-title').innerText = `Update Tank Record — ${item.tankNo}`;
        const btn = document.querySelector('#panel-tankmaint .btn-primary[onclick="submitTankMaintenance()"]');
        if (btn) {
            btn.innerText = 'Update Tank Record';
            btn.setAttribute('data-edit-uid', uid);
            btn.setAttribute('onclick', `updateTankMaintenance('${uid}')`);
        }
        document.getElementById('tank-cancel-edit-btn').style.display = '';
        switchTab('tankmaint');
        document.getElementById('tank-form-title').scrollIntoView({ behavior:'smooth', block:'nearest' });
    }

    async function updateTankMaintenance(uid) {
        const tankNo = document.getElementById('tank-no').value.trim();
        if (!tankNo) { alert('Tank number is required.'); return; }
        const payload = {
            tankNo,
            srNo: document.getElementById('tank-sr-no').value.trim(),
            handoverDate: document.getElementById('tank-handover-date').value || '',
            edr: document.getElementById('tank-edr').value || '',
            status: document.getElementById('tank-status').value || 'Out of Commission',
            actualReturnDate: document.getElementById('tank-return-date').value || '',
            remarks: document.getElementById('tank-remarks').value.trim()
        };
        try {
            await api('PUT', `/tank_maintenance/${uid}`, payload);
            tankMaintDb = await api('GET', '/tank_maintenance');
            renderTankBackendTable();
            renderDashboard();
            cancelTankEdit();
        } catch(e) { alert(e.message); }
    }

    function cancelTankEdit() {
        document.getElementById('tank-form-title').innerText = 'Add Tank Maintenance Record';
        ['tank-no','tank-sr-no','tank-handover-date','tank-edr','tank-return-date','tank-remarks'].forEach(id => {
            const el = document.getElementById(id);
            if (el) el.value = '';
        });
        document.getElementById('tank-status').value = 'Out of Commission';
        const btn = document.querySelector('#panel-tankmaint .btn-primary[data-edit-uid]') ||
            document.querySelector('#panel-tankmaint .btn-primary[onclick^="updateTankMaintenance"]');
        if (btn) {
            btn.innerText = 'Save Tank Record';
            btn.removeAttribute('data-edit-uid');
            btn.setAttribute('onclick', 'submitTankMaintenance()');
        }
        document.getElementById('tank-cancel-edit-btn').style.display = 'none';
    }

    async function deleteTankMaintenance(uid) {
        if (!confirm('Delete this tank maintenance record?')) return;
        try {
            await api('DELETE', `/tank_maintenance/${uid}`);
            tankMaintDb = await api('GET', '/tank_maintenance');
            renderTankBackendTable();
            renderDashboard();
        } catch(e) { alert(e.message); }
    }

    async function markTankReturned(uid) {
        const item = tankMaintDb.find(x => x.uid === uid);
        if (!item) return;
        const today = new Date().toISOString().split('T')[0];
        try {
            await api('PUT', `/tank_maintenance/${uid}`, {
                tankNo: item.tankNo,
                srNo: item.srNo || '',
                handoverDate: item.handoverDate || '',
                edr: item.edr || '',
                status: 'Returned',
                actualReturnDate: item.actualReturnDate || today,
                remarks: item.remarks || ''
            });
            tankMaintDb = await api('GET', '/tank_maintenance');
            renderTankBackendTable();
            renderDashboard();
        } catch(e) { alert(e.message); }
    }

    function renderTankBackendTable() {
        const tbody = document.getElementById('tank-backend-table-body');
        if (!tbody) return;
        const filter = document.getElementById('tank-table-filter')?.value || 'active';
        let list = [...tankMaintDb];
        if (filter === 'active') list = list.filter(isTankActive);
        if (filter === 'returned') list = list.filter(item => !isTankActive(item));
        if (!list.length) {
            tbody.innerHTML = `<tr><td colspan="7" style="text-align:center;color:var(--good);padding:20px;font-weight:600;">✓ No tank records in this view</td></tr>`;
            return;
        }
        tbody.innerHTML = list.map(item => {
            const active = isTankActive(item);
            const dueDays = tankDaysToEdr(item);
            const dueText = dueDays === null ? fmtDate(item.edr) :
                dueDays < 0 ? `<span class="status-crit">${fmtDate(item.edr)} (${Math.abs(dueDays)}d overdue)</span>` :
                dueDays === 0 ? `<span class="status-warn">${fmtDate(item.edr)} (today)</span>` :
                `${fmtDate(item.edr)} (${dueDays}d left)`;
            return `<tr>
                <td><strong>${item.tankNo}</strong><br><span style="font-size:11px;color:var(--text-muted);">${item.updatedBy || item.createdBy || '—'}</span></td>
                <td>${item.srNo || '—'}</td>
                <td>${fmtDate(item.handoverDate)}</td>
                <td>${dueText}</td>
                <td><span class="${active ? 'status-warn' : 'status-ok'}" style="font-weight:700;">${item.status || (active ? 'Out of Commission' : 'Returned')}</span></td>
                <td>${fmtDate(item.actualReturnDate)}</td>
                <td style="white-space:nowrap;">
                    <button class="btn-primary" style="padding:3px 8px;font-size:11px;width:auto;margin:0 3px 0 0;" onclick="editTankMaintenance('${item.uid}')">Edit</button>
                    ${active ? `<button class="btn-secondary" style="padding:3px 8px;font-size:11px;width:auto;margin:0 3px 0 0;" onclick="markTankReturned('${item.uid}')">Returned</button>` : ''}
                    <button class="btn-danger" onclick="deleteTankMaintenance('${item.uid}')">Del</button>
                </td>
            </tr>`;
        }).join('');
    }

    function isMothballedRecordActive(item) {
        return (item.status || 'Mothballed') !== 'Reactivated';
    }

    async function submitMothballedEquipment() {
        const assetName = document.getElementById('mothballed-asset-name').value.trim();
        const equipmentTag = document.getElementById('mothballed-equipment-tag').value.trim();
        if (!assetName || !equipmentTag) { alert('Asset name and equipment tag are required.'); return; }
        const payload = {
            uid: Date.now().toString(),
            assetName,
            equipmentTag,
            site: document.getElementById('mothballed-site').value.trim() || '—',
            dept: document.getElementById('mothballed-dept').value || 'Mechanical',
            mothballedOn: document.getElementById('mothballed-on').value || '',
            mothballedBy: document.getElementById('mothballed-by').value.trim() || (currentUser?.role || ''),
            status: document.getElementById('mothballed-status').value || 'Mothballed',
            reactivatedOn: document.getElementById('mothballed-reactivated-on').value || '',
            reactivatedBy: document.getElementById('mothballed-reactivated-by').value.trim(),
            futurePlan: document.getElementById('mothballed-future-plan').value.trim()
        };
        try {
            await api('POST', '/mothballed_equipment', payload);
            mothballedEquipDb = await api('GET', '/mothballed_equipment');
            renderMothballedBackendTable();
            renderDashboard();
            cancelMothballedEdit();
        } catch(e) { alert(e.message); }
    }

    function editMothballedEquipment(uid) {
        const item = mothballedEquipDb.find(x => x.uid === uid);
        if (!item) return;
        document.getElementById('mothballed-asset-name').value = item.assetName || '';
        document.getElementById('mothballed-equipment-tag').value = item.equipmentTag || '';
        document.getElementById('mothballed-site').value = item.site || '';
        document.getElementById('mothballed-dept').value = item.dept || 'Mechanical';
        document.getElementById('mothballed-on').value = item.mothballedOn || '';
        document.getElementById('mothballed-by').value = item.mothballedBy || '';
        document.getElementById('mothballed-status').value = item.status || 'Mothballed';
        document.getElementById('mothballed-reactivated-on').value = item.reactivatedOn || '';
        document.getElementById('mothballed-reactivated-by').value = item.reactivatedBy || '';
        document.getElementById('mothballed-future-plan').value = item.futurePlan || '';
        document.getElementById('mothballed-form-title').innerText = `Update Mothballed Record — ${item.assetName}`;
        const btn = document.querySelector('#panel-mothballed .btn-primary[onclick="submitMothballedEquipment()"]');
        if (btn) {
            btn.innerText = 'Update Mothballed Record';
            btn.setAttribute('data-edit-uid', uid);
            btn.setAttribute('onclick', `updateMothballedEquipment('${uid}')`);
        }
        document.getElementById('mothballed-cancel-edit-btn').style.display = '';
        switchTab('mothballed');
        document.getElementById('mothballed-form-title').scrollIntoView({ behavior:'smooth', block:'nearest' });
    }

    async function updateMothballedEquipment(uid) {
        const assetName = document.getElementById('mothballed-asset-name').value.trim();
        const equipmentTag = document.getElementById('mothballed-equipment-tag').value.trim();
        if (!assetName || !equipmentTag) { alert('Asset name and equipment tag are required.'); return; }
        const payload = {
            assetName,
            equipmentTag,
            site: document.getElementById('mothballed-site').value.trim() || '—',
            dept: document.getElementById('mothballed-dept').value || 'Mechanical',
            mothballedOn: document.getElementById('mothballed-on').value || '',
            mothballedBy: document.getElementById('mothballed-by').value.trim() || '',
            status: document.getElementById('mothballed-status').value || 'Mothballed',
            reactivatedOn: document.getElementById('mothballed-reactivated-on').value || '',
            reactivatedBy: document.getElementById('mothballed-reactivated-by').value.trim(),
            futurePlan: document.getElementById('mothballed-future-plan').value.trim()
        };
        try {
            await api('PUT', `/mothballed_equipment/${uid}`, payload);
            mothballedEquipDb = await api('GET', '/mothballed_equipment');
            renderMothballedBackendTable();
            renderDashboard();
            cancelMothballedEdit();
        } catch(e) { alert(e.message); }
    }

    function cancelMothballedEdit() {
        document.getElementById('mothballed-form-title').innerText = 'Add Mothballed Equipment Record';
        ['mothballed-asset-name','mothballed-equipment-tag','mothballed-site','mothballed-on','mothballed-by','mothballed-reactivated-on','mothballed-reactivated-by','mothballed-future-plan'].forEach(id => {
            const el = document.getElementById(id);
            if (el) el.value = '';
        });
        document.getElementById('mothballed-dept').value = 'Mechanical';
        document.getElementById('mothballed-status').value = 'Mothballed';
        const btn = document.querySelector('#panel-mothballed .btn-primary[data-edit-uid]') ||
            document.querySelector('#panel-mothballed .btn-primary[onclick^="updateMothballedEquipment"]');
        if (btn) {
            btn.innerText = 'Save Mothballed Record';
            btn.removeAttribute('data-edit-uid');
            btn.setAttribute('onclick', 'submitMothballedEquipment()');
        }
        document.getElementById('mothballed-cancel-edit-btn').style.display = 'none';
    }

    async function deleteMothballedEquipment(uid) {
        if (!confirm('Delete this mothballed equipment record?')) return;
        try {
            await api('DELETE', `/mothballed_equipment/${uid}`);
            mothballedEquipDb = await api('GET', '/mothballed_equipment');
            renderMothballedBackendTable();
            renderDashboard();
            cancelMothballedEdit();
        } catch(e) { alert(e.message); }
    }

    async function markMothballedReactivated(uid) {
        const item = mothballedEquipDb.find(x => x.uid === uid);
        if (!item) return;
        try {
            await api('PUT', `/mothballed_equipment/${uid}`, {
                assetName: item.assetName,
                equipmentTag: item.equipmentTag,
                site: item.site || '—',
                dept: item.dept || 'Mechanical',
                mothballedOn: item.mothballedOn || '',
                mothballedBy: item.mothballedBy || '',
                status: 'Reactivated',
                reactivatedOn: item.reactivatedOn || new Date().toISOString().split('T')[0],
                reactivatedBy: item.reactivatedBy || (currentUser?.role || currentUser?.username || ''),
                futurePlan: item.futurePlan || ''
            });
            mothballedEquipDb = await api('GET', '/mothballed_equipment');
            renderMothballedBackendTable();
            renderDashboard();
        } catch(e) { alert(e.message); }
    }

    function renderMothballedBackendTable() {
        const tbody = document.getElementById('mothballed-backend-table-body');
        if (!tbody) return;
        const filter = document.getElementById('mothballed-table-filter')?.value || 'active';
        let list = [...mothballedEquipDb];
        if (filter === 'active') list = list.filter(isMothballedRecordActive);
        if (filter === 'reactivated') list = list.filter(item => !isMothballedRecordActive(item));
        if (!list.length) {
            tbody.innerHTML = `<tr><td colspan="7" style="text-align:center;color:var(--good);padding:20px;font-weight:600;">✓ No mothballed equipment records in this view</td></tr>`;
            return;
        }
        tbody.innerHTML = list.map(item => {
            const active = isMothballedRecordActive(item);
            return `<tr>
                <td><strong>${item.assetName}</strong><br><span style="font-size:11px;color:var(--text-muted);">${item.site || '—'} • ${item.dept || '—'}</span></td>
                <td>${item.equipmentTag || '—'}</td>
                <td>${fmtDate(item.mothballedOn)}</td>
                <td>${item.mothballedBy || '—'}</td>
                <td><span class="${active ? 'status-warn' : 'status-ok'}" style="font-weight:700;">${item.status || (active ? 'Mothballed' : 'Reactivated')}</span></td>
                <td>${active ? '—' : fmtDate(item.reactivatedOn)}</td>
                <td style="white-space:nowrap;">
                    <button class="btn-primary" style="padding:3px 8px;font-size:11px;width:auto;margin:0 3px 0 0;" onclick="editMothballedEquipment('${item.uid}')">Edit</button>
                    ${active ? `<button class="btn-secondary" style="padding:3px 8px;font-size:11px;width:auto;margin:0 3px 0 0;" onclick="markMothballedReactivated('${item.uid}')">Reactivated</button>` : ''}
                    <button class="btn-danger" onclick="deleteMothballedEquipment('${item.uid}')">Del</button>
                </td>
            </tr>`;
        }).join('');
    }

    // ─── AUDIT LOG ───────────────────────────────────────────
    async function loadAuditLog() {
        try {
            auditLog = await api('GET', '/audit');
            renderAuditLog();
        } catch(e) { console.error('Audit log error:', e); }
    }

    function renderAuditLog() {
        const tbody = document.getElementById('audit-table-body'); if(!tbody) return;
        const fUser   = document.getElementById('hist-filter-user')?.value||'';
        const fAction = document.getElementById('hist-filter-action')?.value||'';
        const userSel = document.getElementById('hist-filter-user');
        if(userSel){
            const users=[...new Set(auditLog.map(l=>l.user))];
            userSel.innerHTML='<option value="">All Users</option>'+users.map(u=>`<option value="${u}"${u===fUser?' selected':''}>${u}</option>`).join('');
        }
        let list = auditLog;
        if(fUser)   list=list.filter(l=>l.user===fUser);
        if(fAction) list=list.filter(l=>l.action===fAction);
        if(!list.length){tbody.innerHTML=`<tr><td colspan="5" style="text-align:center;padding:20px;color:var(--text-muted);">No log entries found.</td></tr>`;return;}
        tbody.innerHTML = list.map(l=>{
            const col = l.action.startsWith('ADD')||l.action==='CHANGE_PASS'?'var(--good)':l.action.startsWith('DEL')||l.action==='RESOLVE_ISSUE'?'var(--crit)':'var(--warn)';
            return `<tr>
                <td style="font-family:'DM Mono',monospace;font-size:11px;white-space:nowrap;">${l.ts}</td>
                <td style="font-weight:600;">${l.user}</td>
                <td><span class="role-badge">${l.role}</span></td>
                <td><span style="font-size:11px;font-weight:700;color:${col};">${l.action}</span></td>
                <td style="font-size:12px;color:var(--text-secondary);">${l.detail}</td>
            </tr>`;
        }).join('');
    }

        // ─── DASHBOARD RENDERING ───
    let activeDeptFilter = null;
    let activeSiteFilter = null;

    function setOpsPage(page) {
        activeOpsPage = ['equipment', 'tank', 'mothballed'].includes(page) ? page : 'equipment';
        renderDashboard();
    }

    function isMothballedActive(item) {
        return !item.reactivatedOn;
    }

    function equipmentDashboardMarkup() {
        return `
            <div class="section-label">Operations Overview</div>
            <div class="kpi-row">
                <div class="kpi-card">
                    <div class="kpi-label">Plant Health Index</div>
                    <div class="health-ring" id="kpi-health-gauge"><span>100%</span></div>
                </div>
                <div class="kpi-card">
                    <div class="kpi-label">Active Breakdowns</div>
                    <div class="kpi-num status-crit" id="kpi-breakdowns">0</div>
                </div>
                <div class="kpi-card kpi-table-card">
                    <div class="kpi-label">Department Summary</div>
                    <table class="summary-table">
                        <thead><tr><th></th><th>⚡ Electrical</th><th>🔧 Mechanical</th><th>🧪 Instrument</th><th>🏗 BM</th></tr></thead>
                        <tbody id="kpi-summary-table"></tbody>
                    </table>
                </div>
            </div>
            <div class="dual-panel">
                <div class="dual-left">
                    <div class="panel-section-label disc">
                        <svg width="13" height="13" viewBox="0 0 24 24" fill="none" stroke="#4338ca" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="12" r="10"/><line x1="12" y1="8" x2="12" y2="12"/><line x1="12" y1="16" x2="12.01" y2="16"/></svg>
                        Discipline Wise
                    </div>
                    <div class="gauge-stack">
                        <div class="gauge-card gauge-card-wide" id="card-elec" onclick="openDeptPanel('Electrical')">
                            <div class="gauge-row-inner">
                                <div class="gauge-left-meta"><div class="gauge-dept-name">⚡ Electrical</div><div class="gauge-hint">View Details ↓</div></div>
                                <div class="semi-gauge-wrap"><div class="gauge-fill" id="gauge-elec"></div><div class="gauge-pct" id="pct-elec">100%</div></div>
                            </div>
                        </div>
                        <div class="gauge-card gauge-card-wide" id="card-mech" onclick="openDeptPanel('Mechanical')">
                            <div class="gauge-row-inner">
                                <div class="gauge-left-meta"><div class="gauge-dept-name">🔧 Mechanical</div><div class="gauge-hint">View Details ↓</div></div>
                                <div class="semi-gauge-wrap"><div class="gauge-fill" id="gauge-mech"></div><div class="gauge-pct" id="pct-mech">100%</div></div>
                            </div>
                        </div>
                        <div class="gauge-card gauge-card-wide" id="card-inst" onclick="openDeptPanel('Instrument')">
                            <div class="gauge-row-inner">
                                <div class="gauge-left-meta"><div class="gauge-dept-name">🧪 Instrument</div><div class="gauge-hint">View Details ↓</div></div>
                                <div class="semi-gauge-wrap"><div class="gauge-fill" id="gauge-inst"></div><div class="gauge-pct" id="pct-inst">100%</div></div>
                            </div>
                        </div>
                        <div class="gauge-card gauge-card-wide" id="card-bm" onclick="openDeptPanel('Building Maintenance')">
                            <div class="gauge-row-inner">
                                <div class="gauge-left-meta"><div class="gauge-dept-name">🏗 BM</div><div class="gauge-hint">View Details ↓</div></div>
                                <div class="semi-gauge-wrap"><div class="gauge-fill" id="gauge-bm"></div><div class="gauge-pct" id="pct-bm">100%</div></div>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="dual-right">
                    <div class="panel-section-label asset">
                        <svg width="13" height="13" viewBox="0 0 24 24" fill="none" stroke="#065f46" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"><rect x="3" y="3" width="7" height="7"/><rect x="14" y="3" width="7" height="7"/><rect x="14" y="14" width="7" height="7"/><rect x="3" y="14" width="7" height="7"/></svg>
                        Asset Wise
                    </div>
                    <div class="site-grid" id="site-container"></div>
                </div>
            </div>
            <div id="master-details-panel" class="details-panel">
                <div class="panel-header">
                    <h2 id="master-panel-title">Details</h2>
                    <button class="close-btn" onclick="closeMasterPanel()">✕</button>
                </div>
                <div id="master-panel-body"></div>
            </div>`;
    }

    function tankDashboardMarkup() {
        const active = tankMaintDb.filter(isTankActive);
        const returned = tankMaintDb.filter(item => !isTankActive(item));
        const overdue = active.filter(item => {
            const dueDays = tankDaysToEdr(item);
            return dueDays !== null && dueDays < 0;
        });
        const activeRows = active.length ? active.map(item => {
            const dueDays = tankDaysToEdr(item);
            const edrText = dueDays === null ? fmtDate(item.edr) :
                dueDays < 0 ? `<span class="status-crit">${fmtDate(item.edr)} (${Math.abs(dueDays)}d overdue)</span>` :
                dueDays === 0 ? `<span class="status-warn">${fmtDate(item.edr)} (today)</span>` :
                `${fmtDate(item.edr)} (${dueDays}d left)`;
            return `<tr>
                <td><strong>${item.tankNo}</strong></td>
                <td>${item.srNo || '—'}</td>
                <td>${fmtDate(item.handoverDate)}</td>
                <td>${edrText}</td>
                <td>${item.remarks || '—'}</td>
            </tr>`;
        }).join('') : `<tr><td colspan="5" style="text-align:center;padding:18px;color:var(--good);font-weight:600;">✓ No tanks currently out of commission</td></tr>`;
        const returnedRows = returned.slice(0, 8).map(item => `<tr>
            <td><strong>${item.tankNo}</strong></td>
            <td>${item.srNo || '—'}</td>
            <td>${fmtDate(item.actualReturnDate)}</td>
            <td>${fmtDate(item.edr)}</td>
            <td>${item.updatedBy || item.createdBy || '—'}</td>
        </tr>`).join('') || `<tr><td colspan="5" style="text-align:center;padding:18px;color:var(--text-muted);">No returned tanks yet</td></tr>`;
        return `
            <div class="section-label">Tank on Maint</div>
            <div class="kpi-row">
                <div class="kpi-card">
                    <div class="kpi-label">Out of Commission</div>
                    <div class="kpi-num status-warn">${active.length}</div>
                </div>
                <div class="kpi-card">
                    <div class="kpi-label">Returned Tanks</div>
                    <div class="kpi-num status-ok">${returned.length}</div>
                </div>
                <div class="kpi-card kpi-table-card">
                    <div class="kpi-label">EDR Watch</div>
                    <table class="summary-table">
                        <tbody>
                            <tr><td>Overdue</td><td class="status-crit">${overdue.length}</td></tr>
                            <tr><td>Due Today</td><td class="status-warn">${active.filter(item => tankDaysToEdr(item) === 0).length}</td></tr>
                            <tr><td>Upcoming</td><td>${active.filter(item => { const d = tankDaysToEdr(item); return d !== null && d > 0; }).length}</td></tr>
                        </tbody>
                    </table>
                </div>
            </div>
            <div class="data-table-card" style="margin-bottom:18px;">
                <div style="display:flex;justify-content:space-between;align-items:center;gap:12px;flex-wrap:wrap;">
                    <h3 style="margin:0;">Out of Commissioned Tanks</h3>
                    ${currentUser ? '<button class="btn-primary" style="width:auto;padding:8px 14px;" onclick="openTankMaintenanceManager()">Manage Records</button>' : ''}
                </div>
                <div style="overflow-x:auto;margin-top:12px;">
                    <table class="issue-tbl">
                        <thead><tr><th>Tank</th><th>SR No.</th><th>Date of Handing Over</th><th>Expected Date of Return (EDR)</th><th>Remarks</th></tr></thead>
                        <tbody>${activeRows}</tbody>
                    </table>
                </div>
            </div>
            <div class="data-table-card">
                <h3 style="margin:0 0 12px 0;">Recently Returned Tanks</h3>
                <div style="overflow-x:auto;">
                    <table class="issue-tbl">
                        <thead><tr><th>Tank</th><th>SR No.</th><th>Return Date</th><th>EDR</th><th>Updated By</th></tr></thead>
                        <tbody>${returnedRows}</tbody>
                    </table>
                </div>
            </div>`;
    }

    function mothballedDashboardMarkup() {
        const active = mothballedEquipDb.filter(isMothballedActive);
        const returned = mothballedEquipDb.filter(item => !isMothballedActive(item))
            .sort((a, b) => String(b.reactivatedOn).localeCompare(String(a.reactivatedOn)));
        const byDept = {
            elec: active.filter(item => item.dept === 'Electrical').length,
            mech: active.filter(item => item.dept === 'Mechanical').length,
            inst: active.filter(item => item.dept === 'Instrument').length,
            bm: active.filter(item => item.dept === 'Building Maintenance').length
        };
        const reviewSoon = active.filter(item => {
            const days = daysSince(item.mothballedOn);
            return Number.isFinite(days) && days >= 120;
        }).length;
        const activeRows = active.length ? active.map(item => `<tr>
            <td><strong>${item.assetName}</strong><div style="font-size:11px;color:var(--text-muted);margin-top:4px;">${item.site} • ${item.dept}</div></td>
            <td>${item.equipmentTag}</td>
            <td>${fmtDate(item.mothballedOn)}</td>
            <td>${item.mothballedBy || '—'}</td>
            <td>${item.futurePlan || '—'}</td>
        </tr>`).join('') : `<tr><td colspan="5" style="text-align:center;padding:18px;color:var(--good);font-weight:600;">✓ No mothballed equipment in the current list</td></tr>`;
        const returnedRows = returned.slice(0, 8).map(item => `<tr>
            <td><strong>${item.assetName}</strong><div style="font-size:11px;color:var(--text-muted);margin-top:4px;">${item.site} • ${item.dept}</div></td>
            <td>${item.equipmentTag}</td>
            <td>${fmtDate(item.reactivatedOn)}</td>
            <td>${item.reactivatedBy || '—'}</td>
            <td>${item.futurePlan || '—'}</td>
        </tr>`).join('') || `<tr><td colspan="5" style="text-align:center;padding:18px;color:var(--text-muted);">No recently reactivated items yet</td></tr>`;
        return `
            <div class="section-label">Mothballed Equipment</div>
            <div class="kpi-row">
                <div class="kpi-card">
                    <div class="kpi-label">Currently Mothballed</div>
                    <div class="kpi-num status-warn">${active.length}</div>
                </div>
                <div class="kpi-card">
                    <div class="kpi-label">Recently Reactivated</div>
                    <div class="kpi-num status-ok">${returned.length}</div>
                </div>
                <div class="kpi-card kpi-table-card">
                    <div class="kpi-label">Review Snapshot</div>
                    <table class="summary-table">
                        <tbody>
                            <tr><td>Electrical</td><td>${byDept.elec}</td></tr>
                            <tr><td>Mechanical</td><td>${byDept.mech}</td></tr>
                            <tr><td>Instrument</td><td>${byDept.inst}</td></tr>
                            <tr><td>Review Due</td><td class="${reviewSoon ? 'status-warn' : ''}">${reviewSoon}</td></tr>
                        </tbody>
                    </table>
                </div>
            </div>
            <div class="data-table-card" style="margin-bottom:18px;">
                <div style="display:flex;justify-content:space-between;align-items:center;gap:12px;flex-wrap:wrap;">
                    <h3 style="margin:0;">Active Mothballed Items</h3>
                    <div style="display:flex;align-items:center;gap:12px;flex-wrap:wrap;">
                        <div style="font-size:12px;color:var(--text-muted);">Track preservation status, ownership and next action</div>
                        ${currentUser ? '<button class="btn-primary" style="width:auto;padding:8px 14px;" onclick="openMothballedManager()">Manage Records</button>' : ''}
                    </div>
                </div>
                <div style="overflow-x:auto;margin-top:12px;">
                    <table class="issue-tbl">
                        <thead><tr><th>Asset Name</th><th>Equipment Tag</th><th>Mothballed On</th><th>Mothballed By</th><th>Future Plan / Remarks</th></tr></thead>
                        <tbody>${activeRows}</tbody>
                    </table>
                </div>
            </div>
            <div class="data-table-card">
                <h3 style="margin:0 0 12px 0;">Recently Reactivated Items</h3>
                <div style="overflow-x:auto;">
                    <table class="issue-tbl">
                        <thead><tr><th>Asset Name</th><th>Equipment Tag</th><th>Reactivated On</th><th>Reactivated By</th><th>Remarks</th></tr></thead>
                        <tbody>${returnedRows}</tbody>
                    </table>
                </div>
            </div>`;
    }

    function renderDashboard() {
        const wrapper = document.getElementById('ops-page-content');
        if (wrapper) {
            const equipmentTab = document.getElementById('ops-tab-equipment');
            const tankTab = document.getElementById('ops-tab-tank');
            const mothballedTab = document.getElementById('ops-tab-mothballed');
            if (equipmentTab) equipmentTab.classList.toggle('active', activeOpsPage === 'equipment');
            if (tankTab) tankTab.classList.toggle('active', activeOpsPage === 'tank');
            if (mothballedTab) mothballedTab.classList.toggle('active', activeOpsPage === 'mothballed');
            wrapper.innerHTML =
                activeOpsPage === 'tank' ? tankDashboardMarkup() :
                activeOpsPage === 'mothballed' ? mothballedDashboardMarkup() :
                equipmentDashboardMarkup();
            if (activeOpsPage !== 'equipment') return;
        }
        closeMasterPanel();
        let grandTotal = {elec:0,mech:0,inst:0,bm:0};
        let failTotal  = {elec:0,mech:0,inst:0,bm:0};
        const siteIssueCounts = {};
        const siteHasCritical = {};

        sitesDb.forEach(s => { grandTotal.elec += s.elec; grandTotal.mech += s.mech; grandTotal.inst += s.inst; grandTotal.bm += (s.bm||0); });
        issuesDb.forEach(i => {
            if(i.dept === 'Electrical')          failTotal.elec++;
            if(i.dept === 'Mechanical')          failTotal.mech++;
            if(i.dept === 'Instrument')          failTotal.inst++;
            if(i.dept === 'Building Maintenance') failTotal.bm++;
            siteIssueCounts[i.site] = (siteIssueCounts[i.site] || 0) + 1;
            if(i.level === 'crit') siteHasCritical[i.site] = true;
        });

        const tFails = failTotal.elec + failTotal.mech + failTotal.inst + failTotal.bm;
        const tEquip = grandTotal.elec + grandTotal.mech + grandTotal.inst + grandTotal.bm;
        const hlth   = tEquip > 0 ? ((tEquip - tFails) / tEquip) * 100 : 100;

        document.getElementById('kpi-breakdowns').innerText = tFails;

        const gaugeEl = document.getElementById('kpi-health-gauge');
        gaugeEl.querySelector('span').innerText = hlth.toFixed(1) + '%';
        const gc = hlth < 95 ? 'conic-gradient(var(--crit) '+hlth+'%, var(--bg) 0)' :
                   hlth < 98 ? 'conic-gradient(var(--warn) '+hlth+'%, var(--bg) 0)' :
                               'conic-gradient(var(--good) '+hlth+'%, var(--bg) 0)';
        gaugeEl.style.background = gc;
        gaugeEl.querySelector('span').style.color = hlth < 95 ? 'var(--crit)' : hlth < 98 ? 'var(--warn)' : 'var(--good)';

        document.getElementById('kpi-summary-table').innerHTML = `
            <tr>
                <td>Total Equipment</td>
                <td>${grandTotal.elec}</td>
                <td>${grandTotal.mech}</td>
                <td>${grandTotal.inst}</td>
                <td>${grandTotal.bm}</td>
            </tr>
            <tr>
                <td>Unavailable</td>
                <td class="status-crit">${failTotal.elec}</td>
                <td class="status-warn">${failTotal.mech}</td>
                <td class="status-crit">${failTotal.inst}</td>
                <td class="status-warn">${failTotal.bm}</td>
            </tr>`;

        const deptBaseColor = { elec: 'var(--elec-col)', mech: 'var(--mech-col)', inst: 'var(--inst-col)', bm: 'var(--bm-col)' };
        const updateGauge = (fail, tot, prefix) => {
            const pct = tot > 0 ? ((tot - fail) / tot) * 100 : 100;
            document.getElementById(`pct-${prefix}`).innerText = pct.toFixed(1) + '%';
            const deg = -45 + (pct * 1.8);
            const col = pct < 95 ? 'var(--crit)' : pct < 98 ? 'var(--warn)' : deptBaseColor[prefix];
            const fill = document.getElementById(`gauge-${prefix}`);
            fill.style.transform      = `rotate(${deg}deg)`;
            fill.style.borderColor    = col;
            fill.style.borderTopColor = col;
            fill.style.borderRightColor = col;
            document.getElementById(`pct-${prefix}`).style.color = col;
        };
        updateGauge(failTotal.elec, grandTotal.elec, 'elec');
        updateGauge(failTotal.mech, grandTotal.mech, 'mech');
        updateGauge(failTotal.inst, grandTotal.inst, 'inst');
        updateGauge(failTotal.bm,   grandTotal.bm,   'bm');

        // Sites
        const container = document.getElementById('site-container');
        const siteCards = sitesDb.map(site => {
            const sTotal = site.elec + site.mech + site.inst + (site.bm||0);
            const sFail  = siteIssueCounts[site.id] || 0;
            const hasCrit = !!siteHasCritical[site.id];
            const sColor = sFail > 0 ? (hasCrit ? 'var(--crit)' : 'var(--warn)') : 'var(--good)';
            let fPct = sTotal > 0 ? (sFail / sTotal) * 100 : 0;
            if(sFail > 0 && fPct < 5) fPct = 5;

            return `
                <div class="site-card" id="site-card-${site.id}" onclick="openSitePanel('${site.id}')">
                    <div class="site-name">${site.id}</div>
                    <svg viewBox="0 0 36 36" class="circular-chart">
                        <path class="circle-bg" d="M18 2.0845 a 15.9155 15.9155 0 0 1 0 31.831 a 15.9155 15.9155 0 0 1 0 -31.831"/>
                        <path class="circle" stroke-dasharray="${fPct}, 100" d="M18 2.0845 a 15.9155 15.9155 0 0 1 0 31.831 a 15.9155 15.9155 0 0 1 0 -31.831" style="stroke:${sColor};"/>
                        <text x="18" y="17" style="fill:${sColor};font-size:9px;font-weight:700;text-anchor:middle;font-family:sans-serif;">${sFail}</text>
                        <line x1="12" y1="20" x2="24" y2="20" style="stroke:#ddd;stroke-width:0.5;"/>
                        <text x="18" y="27" style="fill:#aaa;font-size:5.5px;text-anchor:middle;font-family:sans-serif;">${sTotal} total</text>
                    </svg>
                </div>`;
        });

        // Fill remaining grid space with ghost placeholder cards
        const TARGET_CELLS = 16;
        const ghostCount = Math.max(0, TARGET_CELLS - sitesDb.length);
        for(let i = 0; i < ghostCount; i++) {
            siteCards.push(`<div class="site-card-ghost"></div>`);
        }
        container.innerHTML = siteCards.join('');
    }

    // ─── HELPERS ───
    function daysSince(dateStr) {
        const d = new Date(dateStr);
        const today = new Date();
        today.setHours(0,0,0,0);
        return Math.max(0, Math.floor((today - d) / 86400000));
    }

    function daysBadge(days) {
        const cls = days >= 14 ? 'status-crit' : days >= 7 ? 'status-warn' : 'status-ok';
        return `<span class="days-badge days-badge-${cls}">${days}d</span>`;
    }

    // ─── ISSUE TABLE ROWS — click opens modal directly ───
    function generateIssueTableRows(issues) {
        if(!issues.length) return `
            <tr><td colspan="6" style="text-align:center;padding:20px;">
                <div class="empty-state">✓ No active issues</div>
            </td></tr>`;

        return issues.map(issue => {
            const isCrit  = issue.level === 'crit';
            const bdrCol  = isCrit ? 'var(--crit)' : 'var(--warn)';
            const badgeCls= isCrit ? 'badge-crit' : 'badge-warn';
            const icon    = isCrit ? '🔴' : '⚠️';
            const days    = daysSince(issue.date);
            const dBadge  = daysBadge(days);
            const etr     = issue.etr || '—';

            return `
            <tr class="issue-table-row" onclick="openModal('${issue.uid}')" style="border-left:3px solid ${bdrCol}; cursor:pointer;" title="Click for full details">
                <td style="padding:10px 12px;">
                    <div style="font-weight:600;font-size:12px;font-family:'DM Mono',monospace;color:var(--accent);">${issue.id}</div>
                    <div style="font-size:11px;color:var(--text-muted);margin-top:2px;">${issue.site}</div>
                </td>
                <td style="padding:10px 12px;">
                    <div style="font-weight:600;font-size:13px;">${issue.name}</div>
                    <div style="font-size:11px;color:var(--text-muted);">${issue.dept}</div>
                </td>
                <td style="padding:10px 12px;text-align:center;">
                    <span class="issue-badge ${badgeCls}">${icon} ${issue.status}</span>
                </td>
                <td style="padding:10px 12px;text-align:center;font-family:'DM Mono',monospace;font-size:12px;">${issue.date}</td>
                <td style="padding:10px 12px;text-align:center;">${dBadge}</td>
                <td style="padding:10px 12px;text-align:center;font-family:'DM Mono',monospace;font-size:12px;color:var(--text-secondary);">${etr}</td>
            </tr>`;
        }).join('');
    }

    function issueTable(issues) {
        return `
        <div style="overflow-x:auto;">
        <table class="issue-tbl">
            <thead>
                <tr>
                    <th>Tag / Site</th>
                    <th>Equipment</th>
                    <th>Status</th>
                    <th>Failed On</th>
                    <th>Days Down</th>
                    <th>Exp. Return</th>
                </tr>
            </thead>
            <tbody>${generateIssueTableRows(issues)}</tbody>
        </table>
        </div>`;
    }

    function closeMasterPanel() {
        document.getElementById('master-details-panel').style.display = 'none';
        ['elec','mech','inst','bm'].forEach(d => document.getElementById('card-'+d).classList.remove('active'));
        sitesDb.forEach(s => { const el = document.getElementById('site-card-'+s.id); if(el) el.classList.remove('active'); });
        activeDeptFilter = null; activeSiteFilter = null;
    }

    function openDeptPanel(deptName, options = {}) {
        const shouldScroll = options.scroll !== false;
        closeMasterPanel();
        const prefix = { Electrical:'elec', Mechanical:'mech', Instrument:'inst', 'Building Maintenance':'bm' };
        document.getElementById('card-' + prefix[deptName]).classList.add('active');
        activeDeptFilter = deptName;

        const panel      = document.getElementById('master-details-panel');
        document.getElementById('master-panel-title').innerText = `${deptName} Department — Active Issues`;

        const deptIssues = issuesDb.filter(x => x.dept === deptName);

        // Compact downtime drivers (build from actual status types)
        const statusCounts = {};
        deptIssues.forEach(i => { statusCounts[i.status] = (statusCounts[i.status]||0)+1; });
        const total = deptIssues.length || 1;
        const driverRows = Object.entries(statusCounts)
            .sort((a,b) => b[1]-a[1]).slice(0,4)
            .map(([s,c]) => {
                const pct = Math.round((c/total)*100);
                const col = s.toLowerCase().includes('seal') || s.toLowerCase().includes('trip') || s.toLowerCase().includes('fail')
                    ? 'var(--crit)' : s.toLowerCase().includes('maint') || s.toLowerCase().includes('vibr')
                    ? 'var(--warn)' : 'var(--accent)';
                return `<div class="bar-row"><div class="bar-label" style="width:160px;font-size:11px;">${s}</div><div class="bar-track"><div class="bar-fill" style="width:${pct}%;background:${col};"></div></div><div class="bar-value">${c} issue${c>1?'s':''}</div></div>`;
            }).join('') || '<div style="font-size:12px;color:var(--text-muted);padding:8px 0;">No issues recorded.</div>';

        document.getElementById('master-panel-body').innerHTML = `
            <div class="panel-split">
                <div class="panel-split-left">
                    <div class="chart-title">Failure Type Breakdown</div>
                    <div style="margin-top:6px;">${driverRows}</div>
                    <div style="margin-top:16px; padding:10px 14px; background:var(--bg); border-radius:6px; border:1px solid var(--border);">
                        <div style="font-size:11px;color:var(--text-muted);font-weight:600;text-transform:uppercase;letter-spacing:.6px;margin-bottom:8px;">Summary</div>
                        <div style="display:flex;gap:18px;">
                            <div style="text-align:center;"><div style="font-size:22px;font-weight:700;font-family:'DM Mono',monospace;color:var(--crit);">${deptIssues.filter(i=>i.level==='crit').length}</div><div style="font-size:10px;color:var(--text-muted);">CRITICAL</div></div>
                            <div style="text-align:center;"><div style="font-size:22px;font-weight:700;font-family:'DM Mono',monospace;color:var(--warn);">${deptIssues.filter(i=>i.level==='warn').length}</div><div style="font-size:10px;color:var(--text-muted);">WARNING</div></div>
                            <div style="text-align:center;"><div style="font-size:22px;font-weight:700;font-family:'DM Mono',monospace;color:var(--text-secondary);">${deptIssues.length}</div><div style="font-size:10px;color:var(--text-muted);">TOTAL</div></div>
                        </div>
                    </div>
                </div>
                <div class="panel-split-right">
                    <div class="chart-title" style="margin-bottom:10px;">Active Issues <span style="color:var(--text-muted);font-weight:400;">(${deptIssues.length})</span></div>
                    ${issueTable(deptIssues)}
                </div>
            </div>`;

        panel.style.display = 'block';
        if (shouldScroll) setTimeout(() => panel.scrollIntoView({ behavior:'smooth', block:'end' }), 50);
    }

    function openSitePanel(siteId, deptFilter = 'All', options = {}) {
        const shouldScroll = options.scroll !== false;
        if(activeSiteFilter !== siteId) closeMasterPanel();
        activeSiteFilter = siteId;
        document.getElementById(`site-card-${siteId}`).classList.add('active');

        const panel       = document.getElementById('master-details-panel');
        const siteDataObj = sitesDb.find(s => s.id === siteId);
        const sIssues     = issuesDb.filter(x => x.site === siteId);

        document.getElementById('master-panel-title').innerText = `${siteId} — Asset Issues`;

        const renderFilterBox = (name) => {
            const failCount = sIssues.filter(x => x.dept === name).length;
            const totCount  = name === 'Electrical'          ? siteDataObj.elec :
                              name === 'Mechanical'           ? siteDataObj.mech :
                              name === 'Instrument'           ? siteDataObj.inst :
                              (siteDataObj.bm||0);
            const hasCrit   = sIssues.some(x => x.dept === name && x.level === 'crit');
            const numCls    = failCount > 0 ? (hasCrit ? 'status-crit' : 'status-warn') : '';
            const active    = deptFilter === name ? 'active' : '';
            return `
                <div class="dept-stat-box ${active}" onclick="openSitePanel('${siteId}', '${deptFilter === name ? 'All' : name}')">
                    <h4>${name}</h4>
                    <div class="numbers"><span class="${numCls}">${failCount}</span> <span style="color:var(--text-muted);font-size:14px;">/ ${totCount}</span></div>
                    <div class="click-hint">Filter ▼</div>
                </div>`;
        };

        const dispIssues = deptFilter === 'All' ? sIssues : sIssues.filter(x => x.dept === deptFilter);

        document.getElementById('master-panel-body').innerHTML = `
            <div class="site-filter-row" style="margin-bottom:16px;">${renderFilterBox('Electrical')}${renderFilterBox('Mechanical')}${renderFilterBox('Instrument')}${renderFilterBox('Building Maintenance')}</div>
            <div class="chart-title" style="margin-bottom:10px;">Active Issues <span style="color:var(--text-muted);font-weight:400;">(${dispIssues.length})</span></div>
            ${issueTable(dispIssues)}`;

        panel.style.display = 'block';
        if(shouldScroll && deptFilter === 'All') setTimeout(() => panel.scrollIntoView({ behavior:'smooth', block:'end' }), 50);
    }

    // ─── MODAL ───
    function openModal(uid) {
        const issue = issuesDb.find(x => x.uid === uid);
        if(!issue) return;
        document.getElementById('modal-equip-name').innerText = issue.name;
        document.getElementById('modal-equip-id').innerText   = `${issue.id}  |  Site: ${issue.site}`;
        document.getElementById('modal-dept').innerText       = issue.dept;
        const statusEl = document.getElementById('modal-status');
        statusEl.innerText    = issue.status;
        statusEl.style.color  = issue.level === 'crit' ? 'var(--crit)' : 'var(--warn)';
        document.getElementById('modal-update').innerText  = issue.update;
        document.getElementById('modal-action').innerText  = issue.action;
        document.getElementById('modal-remarks').innerText = issue.remarks;
        document.getElementById('modal-date').innerText    = issue.date;
        document.getElementById('modal-etr').innerText     = issue.etr || '—';
        document.getElementById('item-modal').style.display = 'flex';
    }

    function closeModal(force = false) {
        if(force || event.target.id === 'item-modal')
            document.getElementById('item-modal').style.display = 'none';
    }

    // ─── EXPORT MENU ────────────────────────────────────────
    function toggleExportMenu(e) {
        e.stopPropagation();
        const dd = document.getElementById('export-dropdown-topbar') || document.getElementById('export-dropdown');
        if (dd) dd.style.display = dd.style.display === 'none' ? 'block' : 'none';
    }
    document.addEventListener('click', () => {
        ['export-dropdown','export-dropdown-topbar'].forEach(id => {
            const dd = document.getElementById(id);
            if (dd) dd.style.display = 'none';
        });
    });

    // ─── DAILY REPORT: PRINT ────────────────────────────────
    function exportDailyReport() {
        document.getElementById('export-dropdown').style.display = 'none';
        const now = new Date();
        const dateStr  = now.toLocaleDateString('en-GB', { day:'2-digit', month:'long', year:'numeric' });
        const timeStr  = now.toLocaleTimeString('en-GB', { hour:'2-digit', minute:'2-digit' });
        const dayOfWeek = now.toLocaleDateString('en-GB', { weekday:'long' });

        let grandTotal = { elec:0, mech:0, inst:0, bm:0 };
        sitesDb.forEach(s => { grandTotal.elec += s.elec; grandTotal.mech += s.mech; grandTotal.inst += s.inst; grandTotal.bm += (s.bm||0); });
        const failTotal = {
            elec: issuesDb.filter(i => i.dept === 'Electrical').length,
            mech: issuesDb.filter(i => i.dept === 'Mechanical').length,
            inst: issuesDb.filter(i => i.dept === 'Instrument').length,
            bm:   issuesDb.filter(i => i.dept === 'Building Maintenance').length,
        };
        const tFails = failTotal.elec + failTotal.mech + failTotal.inst + failTotal.bm;
        const tEquip = grandTotal.elec + grandTotal.mech + grandTotal.inst + grandTotal.bm;
        const hlth   = tEquip > 0 ? ((tEquip - tFails) / tEquip * 100).toFixed(1) : '100.0';
        const hlthColor = parseFloat(hlth) < 95 ? '#dc2626' : parseFloat(hlth) < 98 ? '#d97706' : '#059669';

        const siteRows = sitesDb.map(s => {
            const sIssues  = issuesDb.filter(x => x.site === s.id);
            const sAvail   = (s.elec + s.mech + s.inst + (s.bm||0)) - sIssues.length;
            const sCrit    = sIssues.filter(i => i.level === 'crit').length;
            const sWarn    = sIssues.filter(i => i.level === 'warn').length;
            const sColor   = sIssues.length > 0 ? (sCrit > 0 ? '#fde8e8' : '#fef3e2') : '#e8f8f2';
            return `<tr style="background:${sColor};">
                <td style="font-weight:700;padding:6px 10px;">${s.id}</td>
                <td style="text-align:center;padding:6px 10px;">${s.elec+s.mech+s.inst+(s.bm||0)}</td>
                <td style="text-align:center;padding:6px 10px;">${sAvail}</td>
                <td style="text-align:center;padding:6px 10px;color:#dc2626;font-weight:700;">${sCrit}</td>
                <td style="text-align:center;padding:6px 10px;color:#d97706;font-weight:700;">${sWarn}</td>
            </tr>`;
        }).join('');

        const issueRows = issuesDb.length
            ? issuesDb.map(i => {
                const days     = daysSince(i.date);
                const lvlColor = i.level === 'crit' ? '#dc2626' : '#d97706';
                const daysColor= days >= 14 ? '#dc2626' : days >= 7 ? '#d97706' : '#059669';
                return `<tr>
                    <td style="font-family:monospace;font-size:11px;padding:6px 10px;color:#1a56db;">${i.id}</td>
                    <td style="font-weight:600;padding:6px 10px;">${i.name}</td>
                    <td style="padding:6px 10px;">${i.site}</td>
                    <td style="padding:6px 10px;">${i.dept}</td>
                    <td style="padding:6px 10px;color:${lvlColor};font-weight:700;">${i.status}</td>
                    <td style="padding:6px 10px;font-family:monospace;">${i.date}</td>
                    <td style="text-align:center;padding:6px 10px;font-weight:700;color:${daysColor};">${days}d</td>
                    <td style="padding:6px 10px;font-family:monospace;">${i.etr || '—'}</td>
                    <td style="padding:6px 10px;font-size:11px;color:#555;">${(i.update || '—').substring(0, 80)}${(i.update||'').length > 80 ? '…' : ''}</td>
                    <td style="padding:6px 10px;font-size:11px;color:#555;">${(i.action || '—').substring(0, 80)}${(i.action||'').length > 80 ? '…' : ''}</td>
                </tr>`;
              }).join('')
            : `<tr><td colspan="10" style="text-align:center;padding:20px;color:#059669;font-weight:700;">No active issues — All equipment operational</td></tr>`;

        const html = `<!DOCTYPE html>
<html lang="en"><head>
<meta charset="UTF-8">
<title>EMT Daily Report – ${dateStr}</title>
<style>
    body { font-family: 'Segoe UI', Arial, sans-serif; margin: 0; padding: 20px 32px; color: #111; font-size: 13px; }
    h1 { font-size: 20px; margin: 0; letter-spacing: -0.3px; }
    h2 { font-size: 13px; font-weight: 600; text-transform: uppercase; letter-spacing: 1px; color: #555; margin: 20px 0 8px; border-bottom: 2px solid #e5e7eb; padding-bottom: 4px; }
    .header { display: flex; justify-content: space-between; align-items: flex-start; border-bottom: 3px solid #1a56db; padding-bottom: 14px; margin-bottom: 20px; }
    .header-left { }
    .header-right { text-align: right; font-size: 12px; color: #555; }
    .kpi-row { display: flex; gap: 16px; margin-bottom: 20px; }
    .kpi-box { flex: 1; border: 1px solid #e5e7eb; border-radius: 8px; padding: 14px 18px; text-align: center; background: #f9fafb; }
    .kpi-label { font-size: 11px; text-transform: uppercase; letter-spacing: .8px; color: #6b7280; margin-bottom: 6px; font-weight: 600; }
    .kpi-value { font-size: 28px; font-weight: 700; font-family: monospace; }
    table { width: 100%; border-collapse: collapse; font-size: 12px; margin-bottom: 20px; }
    th { background: #f3f4f6; padding: 7px 10px; text-align: left; font-size: 10px; text-transform: uppercase; letter-spacing: .7px; color: #6b7280; border-bottom: 2px solid #e5e7eb; }
    td { border-bottom: 1px solid #f3f4f6; vertical-align: top; }
    tr:last-child td { border-bottom: none; }
    .footer { margin-top: 24px; border-top: 1px solid #e5e7eb; padding-top: 10px; font-size: 11px; color: #9ca3af; display:flex; justify-content:space-between; }
    @media print { body { padding: 10px 16px; } .no-print { display:none; } }
</style>
</head><body>
<div class="header">
    <div class="header-left">
        <div style="font-size:11px;font-weight:700;letter-spacing:1.5px;color:#1a56db;margin-bottom:4px;">EXP OPS — EQUIPMENT MANAGEMENT TEAM</div>
        <h1>Daily Equipment Availability Report</h1>
        <div style="font-size:12px;color:#555;margin-top:4px;">${dayOfWeek}, ${dateStr}</div>
    </div>
    <div class="header-right">
        <div>Generated: ${timeStr} hrs</div>
        <div style="margin-top:4px;font-size:11px;color:#9ca3af;">pme.kockw.com/emt.html</div>
    </div>
</div>

<div class="kpi-row">
    <div class="kpi-box">
        <div class="kpi-label">Plant Health Index</div>
        <div class="kpi-value" style="color:${hlthColor};">${hlth}%</div>
    </div>
    <div class="kpi-box">
        <div class="kpi-label">Active Breakdowns</div>
        <div class="kpi-value" style="color:#dc2626;">${tFails}</div>
    </div>
    <div class="kpi-box">
        <div class="kpi-label">⚡ Electrical</div>
        <div class="kpi-value" style="color:#2563eb;">${failTotal.elec} / ${grandTotal.elec}</div>
        <div style="font-size:10px;color:#9ca3af;margin-top:2px;">Issues / Total</div>
    </div>
    <div class="kpi-box">
        <div class="kpi-label">&#128295; Mechanical</div>
        <div class="kpi-value" style="color:#d97706;">${failTotal.mech} / ${grandTotal.mech}</div>
        <div style="font-size:10px;color:#9ca3af;margin-top:2px;">Issues / Total</div>
    </div>
    <div class="kpi-box">
        <div class="kpi-label">&#129514; Instrument</div>
        <div class="kpi-value" style="color:#059669;">${failTotal.inst} / ${grandTotal.inst}</div>
        <div style="font-size:10px;color:#9ca3af;margin-top:2px;">Issues / Total</div>
    </div>
</div>

<h2>Site Summary</h2>
<table>
    <thead><tr><th>Site</th><th>Total Equip.</th><th>Available</th><th style="color:#dc2626;">Critical</th><th style="color:#d97706;">Warning</th></tr></thead>
    <tbody>${siteRows}</tbody>
</table>

<h2>Active Issue Register (${issuesDb.length} record${issuesDb.length !== 1 ? 's' : ''})</h2>
<table>
    <thead><tr><th>Tag / ID</th><th>Equipment Name</th><th>Site</th><th>Dept</th><th>Status</th><th>Failed On</th><th>Days Down</th><th>Exp. Return</th><th>Issue Description</th><th>Action Taken</th></tr></thead>
    <tbody>${issueRows}</tbody>
</table>

<div class="footer">
    <span>EMT Equipment Dashboard — Export Operations | KOC</span>
    <span>Report generated ${dateStr} at ${timeStr} hrs</span>
</div>

<div class="no-print" style="text-align:center;margin-top:20px;">
    <button onclick="window.print()" style="padding:10px 24px;background:#1a56db;color:white;border:none;border-radius:6px;font-size:14px;font-weight:600;cursor:pointer;">Print / Save as PDF</button>
</div>
</body></html>`;

        const w = window.open('', '_blank');
        w.document.write(html);
        w.document.close();
        w.focus();
    }

    // ─── DAILY REPORT: CSV DOWNLOAD ─────────────────────────
    function exportCSV() {
        document.getElementById('export-dropdown').style.display = 'none';
        const headers = ['Tag/ID','Equipment Name','Site','Department','Status','Level','Failed On','Days Down','Exp. Return','Issue Description','Action Taken','Remarks','Posted By'];
        const rows = issuesDb.map(i => [
            i.id, i.name, i.site, i.dept, i.status,
            i.level === 'crit' ? 'Critical' : 'Warning',
            i.date, daysSince(i.date), i.etr || '',
            i.update || '', i.action || '', i.remarks || '', i.postedBy || ''
        ]);
        const csv = [headers, ...rows]
            .map(row => row.map(cell => `"${String(cell).replace(/"/g, '""')}"`).join(','))
            .join('\r\n');
        const blob = new Blob(['\uFEFF' + csv], { type: 'text/csv; charset=utf-8' });
        const url  = URL.createObjectURL(blob);
        const a    = document.createElement('a');
        a.href     = url;
        a.download = `EMT_Report_${new Date().toISOString().split('T')[0]}.csv`;
        document.body.appendChild(a);
        a.click();
        document.body.removeChild(a);
        URL.revokeObjectURL(url);
    }

    // Build the permission matrix UI on load (DOM is ready since script is at bottom)
    buildPermMatrixUI();
    applyRoleDefaults();

    // ─── BOOT ───
    (async () => {
        // Show login gate
        document.getElementById('dashboard-view').innerHTML =
            `<div style="text-align:center;padding:80px;color:var(--text-muted);">
                <div style="font-size:32px;margin-bottom:12px;">🔒</div>
                <div style="font-size:15px;font-weight:600;margin-bottom:8px;">Please login to view the Daily Report</div>
                <div style="font-size:13px;color:#9ca3af;">Click the <strong>Login</strong> button above</div>
             </div>`;

        // Check if already logged in via active session
        try {
            const me = await api('GET', '/me');
            if (me) currentUser = me;
        } catch(e) { console.warn('Session check failed:', e); }

        if (!currentUser) { showLoginWall(); return; }
        showProtectedContent();

        document.getElementById('nav-login-btn').innerHTML =
            `<svg width="13" height="13" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="8" r="4"/><path d="M4 20c0-4 3.6-7 8-7s8 3 8 7"/></svg>
            ${currentUser.username} (${currentUser.role})`;
        if (canAccessBackendPortal(currentUser)) {
            document.getElementById('nav-login-btn').onclick = openBackend;
        }
        // Update topbar with logged-in user
        document.getElementById('topbar-login-btn').style.display = 'none';
        document.getElementById('topbar-user-label').textContent = currentUser.username + ' (' + displayRoleLabel(currentUser.role) + ')';
        document.getElementById('topbar-user-pill').style.display = 'flex';
        const canEntry = canAccessBackendPortal(currentUser);
        document.getElementById('btn-tb-entry').style.display  = canEntry ? '' : 'none';
        document.getElementById('btn-tb-export').style.display = '';
        const navAdminInit = document.getElementById('nav-admin');
        if(navAdminInit) navAdminInit.style.display = (currentUser.role==='Admin') ? 'flex' : 'none';

        // Load data only if logged in
        await loadDashboardData();

        // Restore dashboard HTML and render
        document.getElementById('dashboard-view').innerHTML = `
            <div style="display:flex;justify-content:space-between;align-items:center;gap:12px;flex-wrap:wrap;margin-bottom:18px;">
                <div class="section-label" style="margin:0;">Export Operations Dashboard</div>
                <div style="display:flex;gap:8px;flex-wrap:wrap;">
                    <button class="btn-secondary" id="ops-tab-equipment" onclick="setOpsPage('equipment')" style="width:auto;padding:8px 14px;">Equipment Status</button>
                    <button class="btn-secondary" id="ops-tab-tank" onclick="setOpsPage('tank')" style="width:auto;padding:8px 14px;">Tank on Maint</button>
                    <button class="btn-secondary" id="ops-tab-mothballed" onclick="setOpsPage('mothballed')" style="width:auto;padding:8px 14px;">Mothballed Equip.</button>
                </div>
            </div>`;
        document.getElementById('dashboard-view').innerHTML += `<div id="ops-page-content"></div>`;

        renderDashboard();
        if (activeOpsPage === 'equipment') openDeptPanel('Electrical', { scroll: false });

        // Auto-refresh dashboard every 60 seconds so all viewers see live data
        setInterval(async () => {
            try {
                await loadDashboardData();
                renderDashboard();
                // Reopen active panel if one was selected
                if (activeOpsPage === 'equipment') {
                    if(activeDeptFilter) openDeptPanel(activeDeptFilter, { scroll: false });
                    else if(activeSiteFilter) openSitePanel(activeSiteFilter, 'All', { scroll: false });
                }
            } catch(e) { /* silent fail on refresh */ }
        }, 60000);
    })();
</script>
</body>
</html>
