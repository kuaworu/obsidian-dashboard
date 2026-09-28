const wrap = dv.container.createDiv({ cls: 'komo-header-block' });
const hdr  = wrap.createDiv({ cls: 'komo-header' });

// ── BRAND (left) ─────────────────────────────
const brand = hdr.createDiv({ cls: 'komo-brand' });

const savedTitle  = localStorage.getItem('komo-title')  || 'KOMOREBI';
const savedMantra = localStorage.getItem('komo-mantra') || 'notes, thoughts & things that matter';

const titleEl = brand.createEl('div', {
    cls: 'komo-title',
    attr: { contenteditable: 'true', spellcheck: 'false', 'data-placeholder': 'KOMOREBI' }
});
titleEl.textContent = savedTitle;
titleEl.addEventListener('blur', () => {
    const v = titleEl.textContent.trim();
    if (v) localStorage.setItem('komo-title', v);
});

const mantraEl = brand.createEl('div', {
    cls: 'komo-mantra',
    attr: { contenteditable: 'true', spellcheck: 'false' }
});
mantraEl.textContent = savedMantra;
mantraEl.addEventListener('blur', () => {
    const v = mantraEl.textContent.trim();
    if (v) localStorage.setItem('komo-mantra', v);
});

// ── CLOCK (center) ────────────────────────────
const clk    = hdr.createDiv({ cls: 'komo-clock-block' });
const timeEl = clk.createDiv({ cls: 'komo-time' });
const dateEl = clk.createDiv({ cls: 'komo-date' });

function tick() {
    const now = new Date();
    const pad = n => String(n).padStart(2, '0');
    timeEl.textContent = `${pad(now.getHours())}:${pad(now.getMinutes())}:${pad(now.getSeconds())}`;
    const enDate = now.toLocaleDateString('en-US', { month: 'short', day: 'numeric', year: 'numeric' });
    dateEl.textContent = enDate;
}
tick();
setInterval(tick, 1000);

// ── RIGHT — GREETING + STATS ──────────────────
const rightHdr = hdr.createDiv({ cls: 'komo-header-right' });

const HOUR = new Date().getHours();
const GREETS = [
    { h: [0,5],  jp: 'late',    en: 'Still going?' },
    { h: [5,9],  jp: 'dawn',    en: 'New day, new flow.' },
    { h: [9,13], jp: 'morning', en: 'Peak focus. Engage.' },
    { h: [13,18],jp: 'noon',    en: 'Stay in the flow.' },
    { h: [18,21],jp: 'dusk',    en: 'Review your progress.' },
    { h: [21,24],jp: 'night',   en: 'Wind down. Reflect.' }
];
const G = GREETS.find(g => HOUR >= g.h[0] && HOUR < g.h[1]) || GREETS[5];
const gEl = rightHdr.createDiv({ cls: 'komo-greeting' });
gEl.createEl('span', { cls: 'komo-greet-jp', text: G.jp });
gEl.createEl('span', { cls: 'komo-greet-en', text: G.en });

// Thin sakura divider
wrap.createDiv({ cls: 'komo-divider' });