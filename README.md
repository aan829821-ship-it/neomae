<div class="shop-wrapper panel">
    <div class="shop-header">
        <h3>🛒 متجر نيوما المتقدم</h3>
        <div class="shop-tabs">
            <button class="tab-btn active" onclick="switchShopTab('energy', this)">🔋 طاقة</button>
            <button class="tab-btn" onclick="switchShopTab('gear', this)">👕 ملابس</button>
            <button class="tab-btn" onclick="switchShopTab('rare', this)">💎 نوادر</button>
        </div>
    </div>

    <div id="dynamic-shop-grid" class="shop-grid-fancy">
        </div>
</div>

<style>
/* تصميم التبويبات */
.shop-tabs { display: flex; gap: 10px; margin-bottom: 15px; }
.tab-btn {
    background: #1a1d21; color: #888; border: 1px solid #333;
    padding: 8px 15px; border-radius: 8px; cursor: pointer; transition: 0.3s;
}
.tab-btn.active { background: #00ff88; color: #000; border-color: #00ff88; box-shadow: 0 0 10px rgba(0,255,136,0.2); }

/* شبكة العناصر المتوهجة */
.shop-grid-fancy { display: grid; grid-template-columns: repeat(auto-fill, minmax(140px, 1fr)); gap: 15px; }

.neon-item-card {
    background: rgba(255,255,255,0.02); border: 1px solid rgba(255,255,255,0.05);
    border-radius: 12px; padding: 12px; text-align: center;
    position: relative; transition: 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);
}

.neon-item-card:hover {
    border-color: #00d4ff; transform: scale(1.05);
    background: rgba(0, 212, 255, 0.05); box-shadow: 0 0 20px rgba(0, 212, 255, 0.2);
}

.item-img { width: 70px; height: 70px; margin-bottom: 10px; filter: drop-shadow(0 0 5px rgba(0,0,0,0.5)); }

.buy-neon-btn {
    background: linear-gradient(90deg, #00d4ff, #0055ff); color: white;
    border: none; padding: 6px; width: 100%; border-radius: 6px;
    font-weight: bold; cursor: pointer; margin-top: 8px;
}

/* تأثير العملات المتطايرة */
.particle {
    position: fixed; pointer-events: none;
    width: 15px; height: 15px; background: #ffd700;
    border-radius: 50%; z-index: 9999; box-shadow: 0 0 10px #ffd700;
}
</style>

<script>
// البيانات الموسعة للمتجر
const fullShopData = {
    energy: [
        { id: "s3", name: "حزمة طاقة", price: 120, img: "⚡" },
        { id: "s4", name: "جرعة نيون", price: 300, img: "🧪" }
    ],
    gear: [
        { id: "g1", name: "قبعة مستكشف", price: 500, img: "🤠" },
        { id: "g2", name: "نظارة ليلية", price: 750, img: "🥽" }
    ],
    rare: [
        { id: "r1", name: "بلورة الأساطير", price: 2000, img: "💎" }
    ]
};

// وظيفة تبديل الأقسام مع تأثير بصري
function switchShopTab(category, btnElement) {
    document.querySelectorAll('.tab-btn').forEach(b => b.classList.remove('active'));
    btnElement.classList.add('active');

    const grid = document.getElementById('dynamic-shop-grid');
    grid.style.opacity = 0;
    
    setTimeout(() => {
        grid.innerHTML = '';
        fullShopData[category].forEach(item => {
            grid.innerHTML += `
                <div class="neon-item-card">
                    <div style="font-size: 40px; margin-bottom:10px;">${item.img}</div>
                    <div style="font-size: 0.9em; font-weight:bold;">${item.name}</div>
                    <div style="color:#ffd700; margin:5px 0;">${item.price} $NMA</div>
                    <button class="buy-neon-btn" onclick="executeAdvancedBuy('${item.id}', this)">شراء</button>
                </div>
            `;
        });
        grid.style.opacity = 1;
    }, 200);
}

// وظيفة الشراء مع تأثير انفجار العملات
async function executeAdvancedBuy(itemId, btn) {
    const rect = btn.getBoundingClientRect();
    
    try {
        const res = await fetch(`/action/buy?item_id=${itemId}`);
        const data = await res.json();
        
        if (data.result.includes("❌")) {
            alert(data.result);
        } else {
            // تأثير العملات المتطايرة عند النجاح
            createCoinBurst(rect.left + rect.width/2, rect.top);
            updateUI(data); // تحديث الرصيد والحقيبة
        }
    } catch (e) { console.error("Buy Error:", e); }
}

function createCoinBurst(x, y) {
    for (let i = 0; i < 8; i++) {
        const p = document.createElement('div');
        p.className = 'particle';
        p.style.left = x + 'px';
        p.style.top = y + 'px';
        document.body.appendChild(p);

        const destX = x + (Math.random() - 0.5) * 200;
        const destY = y - 200 - Math.random() * 100;

        p.animate([
            { transform: 'translate(0, 0) scale(1)', opacity: 1 },
            { transform: `translate(${destX - x}px, ${destY - y}px) scale(0)`, opacity: 0 }
        ], { duration: 800, easing: 'ease-out' }).onfinish = () => p.remove();
    }
}

// تشغيل القسم الأول تلقائياً
window.addEventListener('load', () => {
    switchShopTab('energy', document.querySelector('.tab-btn'));
});
</script>
