---
layout: single
title: "Categoria"
permalink: /shop/categoria/abbigliamento/
classes: wide
author_profile: false
shop_category: abbigliamento
shop_category_name: Abbigliamento
---

<style>
.shop-grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(220px,1fr));gap:1.5em;margin-top:1.5em}
.prod-card{background:white;border:1.5px solid #eee;border-radius:12px;padding:1.5em;text-decoration:none;color:#1a1a2e;transition:all .2s;display:block}
.prod-card:hover{border-color:#6c63ff;box-shadow:0 4px 16px rgba(108,99,255,.12);transform:translateY(-2px)}
.prod-card h3{font-size:1em;font-weight:700;margin:0 0 .4em}
.prod-card .price{font-size:1.1em;font-weight:700;color:#6c63ff}
</style>

<p style="margin-bottom:.5em"><a href="{{ '/shop/' | relative_url }}" style="color:#6c63ff;font-size:13px;text-decoration:none">← Torna allo Shop</a></p>
<p style="color:#666;font-size:14px">Prodotti nella categoria <strong>{{ page.shop_category_name }}</strong></p>

<div id="prods-loading" style="color:#aaa;font-size:14px">Caricamento prodotti...</div>
<div class="shop-grid" id="prods-grid"></div>

<script>
const OWNER = 'cialdecompatibili-netizen';
const REPO  = 'cmspush';
const BASE  = 'https://cialdecompatibili-netizen.github.io/cmspush';
const FILTER_CAT = '{{ page.shop_category }}';

function parseFrontmatter(text) {
  const m = text.match(/^---\n([\s\S]*?)\n---/);
  if (!m) return {};
  const obj = {};
  m[1].split('\n').forEach(line => {
    const i = line.indexOf(':');
    if (i < 0) return;
    const k = line.slice(0,i).trim();
    let v = line.slice(i+1).trim().replace(/^["']|["']$/g,'');
    obj[k] = v;
  });
  return obj;
}

async function loadCat() {
  const pGrid = document.getElementById('prods-grid');
  const loading = document.getElementById('prods-loading');
  try {
    const r = await fetch(`https://api.github.com/repos/${OWNER}/${REPO}/contents/_products`);
    if (!r.ok) throw new Error();
    const files = await r.json();
    const mdFiles = files.filter(f => f.name.endsWith('.md'));
    const products = [];
    for (const f of mdFiles) {
      const fr = await fetch(f.download_url);
      const text = await fr.text();
      const meta = parseFrontmatter(text);
      meta._slug = f.name.replace('.md','');
      if (meta.category === FILTER_CAT) products.push(meta);
    }
    loading.style.display = 'none';
    if (products.length === 0) {
      pGrid.innerHTML = '<p style="color:#bbb">Nessun prodotto in questa categoria.</p>';
    } else {
      products.forEach(p => {
        const card = document.createElement('a');
        card.className = 'prod-card';
        card.href = BASE + '/shop/prodotto/?slug=' + p._slug;
        card.innerHTML = `
          ${p.image ? `<img src="${p.image}" alt="${p.title}" style="width:100%;height:140px;object-fit:cover;border-radius:8px;margin-bottom:.8em">` : ''}
          <h3>${p.title||p._slug}</h3>
          <div class="price">€ ${p.price||'—'}</div>
          ${p.stock ? `<div style="font-size:12px;color:#27ae60;margin-top:.3em">✅ ${p.stock} pz</div>` : ''}`;
        pGrid.appendChild(card);
      });
    }
  } catch(e) {
    loading.innerHTML = '<p style="color:#e74c3c">Errore caricamento prodotti.</p>';
  }
}
loadCat();
</script>
