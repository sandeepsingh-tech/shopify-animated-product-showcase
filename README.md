# Animated Product Showcase: High-Performance Shopify Section

### Overview & Core Benefits

The **Animated Product Showcase** is an interactive, conversion-focused product slider engineered specifically for Shopify stores seeking a custom editorial aesthetic without sacrificing speed or usability.

* **Zero App Bloat & Blazing Fast:** Built entirely with lightweight vanilla JavaScript, CSS3 hardware acceleration, and native Liquid markup. No external jQuery, Swiper.js, or heavy third-party app subscriptions required.
* **Fully Native Merchant Customization:** Once installed, non-technical team members and store managers can add, reorder, swap, or remove products instantly directly inside the standard Shopify Theme Customizer without touching code.
* **Device-Optimized Responsive Views:**
* **Desktop:** Displays an expansive 4-product panoramic deck with alternating dynamic tilts (`-2.8°` to `+2.8°`), multi-card directional slide transitions, and an aggressive 360° golden aura hover effect.
* **Mobile:** Automatically transitions to a focused single-product carousel with built-in touchscreen swipe-gesture detection that distinguishes intentional swipes from vertical page scrolling.


* **Aspect-Ratio Resilience:** Uses a bounded image stage (`object-fit: contain`) and strict 2-line title clamping so catalog items with irregular photo shapes or long descriptions remain uniformly aligned.

---

### Step-by-Step Installation Guide

#### Step 1: Create the Section File in Shopify

1. From your Shopify Admin, navigate to **Online Store** $\rightarrow$ **Themes**.
2. On your active theme, click the three-dots menu (**...**) and select **Edit code**.
3. In the left-hand navigation sidebar, locate and expand the **Sections** folder.
4. Click **Add a new section**.
5. Select the **Liquid** option, enter the file name:
```text
animated-product-showcase

```


6. Click **Done** to open the blank editor.

---

#### Step 2: Paste the Complete Section Code

Delete any default boilerplate code present in the file, paste the entire block below, and click **Save** in the top-right corner.

```liquid
{% comment %}
  Animated Product Showcase Section
  - Responsive: 4 products on desktop, 1 on mobile
  - Touchscreen swipe detection on mobile
  - 360-degree golden neon halo
  - Two-line title clamping and uniform product display
{% endcomment %}

<section id="showcase-{{ section.id }}" class="showcase-wrapper">
  <div class="showcase-container">
    
    <!-- Section Header -->
    <header class="showcase-header">
      <div class="showcase-badge-row">
        <span class="showcase-badge-line"></span>
        <span class="showcase-subheading">{{ section.settings.subheading | default: "LIMITED OFFERS" }}</span>
        <span class="showcase-badge-line"></span>
      </div>
      <h2 class="showcase-title">{{ section.settings.heading | default: "FEATURED CURATIONS" }}</h2>
      {% if section.settings.description != blank %}
        <p class="showcase-description">{{ section.settings.description }}</p>
      {% endif %}
    </header>

    <!-- Interactive Stage -->
    <div class="showcase-slider" data-section-id="{{ section.id }}">
      <button type="button" class="showcase-arrow showcase-prev" aria-label="Previous Slide">
        <svg viewBox="0 0 24 24" width="22" height="22" stroke="currentColor" stroke-width="2.5" fill="none" stroke-linecap="round" stroke-linejoin="round">
          <polyline points="15 18 9 12 15 6"></polyline>
        </svg>
      </button>

      <div class="showcase-viewport">
        <div class="showcase-track">
          {% for block in section.blocks %}
            {% assign prod = all_products[block.settings.product] %}
            <div class="showcase-card-item" data-index="{{ forloop.index0 }}" {{ block.shopify_attributes }}>
              <div class="showcase-card-inner">
                <a href="{{ prod.url | default: '#' }}" class="showcase-card-link" aria-label="{{ prod.title | default: 'Featured Item' }}">
                  
                  <div class="showcase-image-box">
                    {% if prod.featured_image != blank %}
                      <img 
                        src="{{ prod.featured_image | image_url: width: 700 }}" 
                        alt="{{ prod.title | escape }}" 
                        loading="lazy" 
                        class="showcase-product-img"
                      />
                    {% elsif block.settings.custom_image != blank %}
                      <img 
                        src="{{ block.settings.custom_image | image_url: width: 700 }}" 
                        alt="{{ block.settings.custom_title | default: 'Product' | escape }}" 
                        loading="lazy" 
                        class="showcase-product-img"
                      />
                    {% else %}
                      {{ 'product-1' | placeholder_svg_tag: 'showcase-placeholder-svg' }}
                    {% endif %}
                  </div>

                  <div class="showcase-info-box">
                    <h3 class="showcase-product-title">
                      {% if prod.title != blank %}
                        {{ prod.title }}
                      {% else %}
                        {{ block.settings.custom_title | default: "PREMIUM SELECTION" }}
                      {% endif %}
                    </h3>

                    <div class="showcase-price-box">
                      <span class="showcase-price">
                        {% if prod.price != blank %}
                          {{ prod.price | money }}
                        {% else %}
                          {{ block.settings.custom_price | default: "₹4,999" }}
                        {% endif %}
                      </span>
                      {% if prod.compare_at_price > prod.price or block.settings.custom_compare_price != blank %}
                        <span class="showcase-compare-price">
                          {% if prod.compare_at_price > prod.price %}
                            {{ prod.compare_at_price | money }}
                          {% else %}
                            {{ block.settings.custom_compare_price }}
                          {% endif %}
                        </span>
                      {% endif %}
                    </div>
                  </div>

                </a>
              </div>
            </div>
          {% endfor %}
        </div>
      </div>

      <button type="button" class="showcase-arrow showcase-next" aria-label="Next Slide">
        <svg viewBox="0 0 24 24" width="22" height="22" stroke="currentColor" stroke-width="2.5" fill="none" stroke-linecap="round" stroke-linejoin="round">
          <polyline points="9 18 15 12 9 6"></polyline>
        </svg>
      </button>
    </div>

    <!-- Pagination Dots -->
    <div class="showcase-dots" aria-label="Slide Indicators"></div>
  </div>
</section>

<style>
  #showcase-{{ section.id }} {
    position: relative;
    background: transparent;
    padding: 30px 16px 45px;
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
    color: #1a1a1a;
    overflow-x: clip;
  }

  .showcase-container {
    max-width: 1400px;
    margin: 0 auto;
    position: relative;
  }

  .showcase-header {
    text-align: center;
    margin-bottom: 24px;
    display: flex;
    flex-direction: column;
    align-items: center;
  }
  .showcase-badge-row {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 12px;
    margin-bottom: 6px;
    width: 100%;
    max-width: 380px;
  }
  .showcase-badge-line {
    flex: 1;
    height: 1.5px;
    background: linear-gradient(90deg, transparent, #e5c158, transparent);
  }
  .showcase-subheading {
    display: inline-flex;
    align-items: center;
    font-size: 0.72rem;
    font-weight: 800;
    letter-spacing: 1.5px;
    text-transform: uppercase;
    color: #b88628;
    background: rgba(229, 193, 88, 0.12);
    border: 1px solid rgba(229, 193, 88, 0.45);
    padding: 3px 12px;
    border-radius: 50px;
  }
  .showcase-title {
    font-size: 1.95rem;
    font-weight: 900;
    letter-spacing: 1.5px;
    color: #111111;
    margin: 0;
    text-transform: uppercase;
    line-height: 1.15;
  }
  .showcase-description {
    font-size: 0.88rem;
    color: #6c757d;
    margin: 5px auto 0;
    max-width: 500px;
    line-height: 1.4;
  }

  .showcase-slider {
    position: relative;
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 12px;
  }
  .showcase-viewport {
    width: 100%;
    max-width: 1320px;
    overflow: hidden;
    padding: 24px 18px;
  }
  .showcase-track {
    display: flex;
    justify-content: center;
    align-items: center;
    gap: clamp(16px, 2vw, 28px);
  }

  .showcase-card-item {
    display: none;
    width: calc(25% - (clamp(16px, 2vw, 28px) * 0.75));
    max-width: 280px;
    flex-shrink: 0;
    transition: transform 0.45s cubic-bezier(0.2, 0.8, 0.25, 1), opacity 0.4s ease;
  }
  .showcase-card-item.is-visible {
    display: block;
    opacity: 1;
    transform: translateX(0);
  }

  .showcase-card-inner {
    border-radius: 18px;
    background: #ffffff;
    border: 2px solid #e5c158;
    box-shadow: 
      0 0 8px rgba(229, 193, 88, 0.4),
      0 0 16px rgba(212, 175, 55, 0.22),
      0 4px 12px rgba(0, 0, 0, 0.04);
    overflow: hidden;
    cursor: pointer;
    transition: 
      transform 0.45s cubic-bezier(0.175, 0.885, 0.32, 1.275), 
      box-shadow 0.35s ease, 
      border-color 0.3s ease;
  }

  .showcase-card-item.pos-0 .showcase-card-inner { transform: rotate(-2.8deg); }
  .showcase-card-item.pos-1 .showcase-card-inner { transform: rotate(1.8deg); }
  .showcase-card-item.pos-2 .showcase-card-inner { transform: rotate(-1.8deg); }
  .showcase-card-item.pos-3 .showcase-card-inner { transform: rotate(2.8deg); }

  .showcase-card-inner:hover,
  .showcase-card-inner:focus-within {
    transform: translateY(-8px) rotate(0deg) scale(1.025) !important;
    border-color: #ffd866;
    box-shadow: 
      0 0 0 2px rgba(255, 216, 102, 0.9),
      0 0 12px rgba(229, 193, 88, 0.75),
      0 0 24px rgba(212, 175, 55, 0.45) !important;
  }

  .showcase-card-link {
    display: flex;
    flex-direction: column;
    text-decoration: none;
    color: inherit;
  }

  .showcase-image-box {
    background: radial-gradient(circle at 50% 45%, #fffdf8 0%, #fef6dd 60%, #fae8b8 100%);
    border-bottom: 1px solid rgba(229, 193, 88, 0.35);
    height: 220px;
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 10px 12px;
    overflow: hidden;
    position: relative;
  }
  .showcase-product-img {
    width: 100%;
    height: 100%;
    object-fit: contain;
    object-position: center;
    border-radius: 6px;
    filter: drop-shadow(0 4px 10px rgba(184, 134, 40, 0.12));
    transition: transform 0.45s cubic-bezier(0.175, 0.885, 0.32, 1.275);
  }
  .showcase-card-inner:hover .showcase-product-img {
    transform: scale(1.05);
  }
  .showcase-placeholder-svg {
    width: 75%;
    height: 75%;
    fill: #c59341;
    opacity: 0.55;
  }

  .showcase-info-box {
    padding: 14px 16px 16px;
    background: #ffffff;
  }
  .showcase-product-title {
    font-size: 0.88rem;
    font-weight: 700;
    letter-spacing: 0.2px;
    color: #181818;
    margin: 0 0 8px;
    text-transform: uppercase;
    line-height: 1.35;
    display: -webkit-box;
    -webkit-line-clamp: 2;
    -webkit-box-orient: vertical;
    overflow: hidden;
    height: 2.7em;
  }
  .showcase-price-box {
    display: flex;
    align-items: baseline;
    gap: 8px;
  }
  .showcase-price {
    font-size: 1.18rem;
    font-weight: 900;
    color: #b88628;
  }
  .showcase-compare-price {
    font-size: 0.88rem;
    color: #8c939c;
    text-decoration: line-through;
  }

  .showcase-arrow {
    background: #ffffff;
    border: 1.5px solid #d4af37;
    color: #111;
    width: 42px;
    height: 42px;
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
    cursor: pointer;
    z-index: 10;
    box-shadow: 0 4px 10px rgba(0, 0, 0, 0.08);
    transition: all 0.25s ease;
    flex-shrink: 0;
  }
  .showcase-arrow:hover {
    background: #e5c158;
    color: #ffffff;
    border-color: #e5c158;
    transform: scale(1.08);
  }

  .showcase-dots {
    display: flex;
    justify-content: center;
    gap: 8px;
    margin-top: 20px;
  }
  .showcase-dot {
    width: 9px;
    height: 9px;
    border-radius: 50%;
    background: #dedede;
    border: none;
    cursor: pointer;
    transition: background 0.3s ease, transform 0.2s ease;
    padding: 0;
  }
  .showcase-dot.is-active {
    background: #b88628;
    transform: scale(1.3);
  }

  @media (min-width: 769px) {
    .exit-slide-next .showcase-card-item.is-visible {
      transform: translateX(-70px);
      opacity: 0;
    }
    .exit-slide-prev .showcase-card-item.is-visible {
      transform: translateX(70px);
      opacity: 0;
    }
    .enter-prep-next .showcase-card-item.is-visible {
      transform: translateX(70px) !important;
      opacity: 0 !important;
      transition: none !important;
    }
    .enter-prep-prev .showcase-card-item.is-visible {
      transform: translateX(-70px) !important;
      opacity: 0 !important;
      transition: none !important;
    }
  }

  @media (max-width: 768px) {
    #showcase-{{ section.id }} {
      padding: 30px 8px 45px;
    }
    .showcase-title {
      font-size: 1.45rem;
    }
    .showcase-slider {
      gap: 6px;
    }
    .showcase-viewport {
      max-width: 100%;
      padding: 20px 16px;
    }
    .showcase-card-item {
      width: 100%;
      max-width: 280px;
    }
    .showcase-image-box {
      height: 240px;
      padding: 12px;
    }
    .showcase-card-item .showcase-card-inner {
      transform: rotate(0deg) scale(1) !important;
      box-shadow: 
        0 0 10px rgba(229, 193, 88, 0.45),
        0 0 20px rgba(212, 175, 55, 0.25),
        0 4px 10px rgba(0, 0, 0, 0.05);
    }
    .showcase-card-inner:active {
      transform: scale(0.99) !important;
      box-shadow: 
        0 0 14px rgba(229, 193, 88, 0.75),
        0 0 26px rgba(212, 175, 55, 0.45) !important;
    }
    .showcase-arrow {
      width: 36px;
      height: 36px;
    }
    .exit-mob-next .showcase-card-item.is-visible {
      transform: translateX(-80px);
      opacity: 0;
    }
    .exit-mob-prev .showcase-card-item.is-visible {
      transform: translateX(80px);
      opacity: 0;
    }
    .enter-mob-prep-next .showcase-card-item.is-visible {
      transform: translateX(80px) !important;
      opacity: 0 !important;
      transition: none !important;
    }
    .enter-mob-prep-prev .showcase-card-item.is-visible {
      transform: translateX(-80px) !important;
      opacity: 0 !important;
      transition: none !important;
    }
  }
</style>

<script>
  (function () {
    function initShowcaseSlider() {
      const sectionRoot = document.getElementById('showcase-{{ section.id }}');
      if (!sectionRoot) return;

      const track = sectionRoot.querySelector('.showcase-track');
      const viewport = sectionRoot.querySelector('.showcase-viewport');
      const cards = Array.from(sectionRoot.querySelectorAll('.showcase-card-item'));
      const prevBtn = sectionRoot.querySelector('.showcase-prev');
      const nextBtn = sectionRoot.querySelector('.showcase-next');
      const dotsContainer = sectionRoot.querySelector('.showcase-dots');

      if (!cards.length) return;

      let currentSlide = 0;
      let isMobile = window.innerWidth <= 768;
      let perView = isMobile ? 1 : 4;
      let totalSlides = Math.ceil(cards.length / perView);
      let isAnimating = false;

      function updateDots() {
        dotsContainer.innerHTML = '';
        totalSlides = Math.ceil(cards.length / perView);
        for (let i = 0; i < totalSlides; i++) {
          const dot = document.createElement('button');
          dot.type = 'button';
          dot.className = 'showcase-dot' + (i === currentSlide ? ' is-active' : '');
          dot.setAttribute('aria-label', 'Go to slide ' + (i + 1));
          dot.addEventListener('click', () => {
            if (i > currentSlide) goToSlide(i, 'next');
            else if (i < currentSlide) goToSlide(i, 'prev');
          });
          dotsContainer.appendChild(dot);
        }
      }

      function renderCards() {
        cards.forEach((card, idx) => {
          const startIndex = currentSlide * perView;
          const endIndex = startIndex + perView;
          
          card.classList.remove('pos-0', 'pos-1', 'pos-2', 'pos-3');

          if (idx >= startIndex && idx < endIndex) {
            card.classList.add('is-visible');
            if (!isMobile) {
              const relPos = idx - startIndex;
              card.classList.add(`pos-${relPos}`);
            }
          } else {
            card.classList.remove('is-visible');
          }
        });

        const dots = dotsContainer.querySelectorAll('.showcase-dot');
        dots.forEach((dot, idx) => {
          dot.classList.toggle('is-active', idx === currentSlide);
        });
      }

      function goToSlide(targetSlide, direction) {
        if (targetSlide === currentSlide || isAnimating) return;
        isAnimating = true;

        const exitClass = isMobile 
          ? (direction === 'next' ? 'exit-mob-next' : 'exit-mob-prev')
          : (direction === 'next' ? 'exit-slide-next' : 'exit-slide-prev');

        const prepClass = isMobile 
          ? (direction === 'next' ? 'enter-mob-prep-next' : 'enter-mob-prep-prev')
          : (direction === 'next' ? 'enter-prep-next' : 'enter-prep-prev');

        track.classList.add(exitClass);

        setTimeout(() => {
          currentSlide = targetSlide;
          track.classList.remove(exitClass);

          renderCards();
          track.classList.add(prepClass);

          requestAnimationFrame(() => {
            requestAnimationFrame(() => {
              track.classList.remove(prepClass);
              setTimeout(() => {
                isAnimating = false;
              }, 450);
            });
          });
        }, 360);
      }

      function handleNext() {
        const target = (currentSlide + 1) % totalSlides;
        goToSlide(target, 'next');
      }

      function handlePrev() {
        const target = (currentSlide - 1 + totalSlides) % totalSlides;
        goToSlide(target, 'prev');
      }

      nextBtn.addEventListener('click', handleNext);
      prevBtn.addEventListener('click', handlePrev);

      // Mobile Touchscreen Swipe Detection
      let startX = 0;
      let startY = 0;
      let distX = 0;
      let distY = 0;
      const threshold = 40;

      viewport.addEventListener('touchstart', (e) => {
        const touch = e.touches[0];
        startX = touch.clientX;
        startY = touch.clientY;
        distX = 0;
        distY = 0;
      }, { passive: true });

      viewport.addEventListener('touchmove', (e) => {
        if (!startX || !startY) return;
        const touch = e.touches[0];
        distX = touch.clientX - startX;
        distY = touch.clientY - startY;
      }, { passive: true });

      viewport.addEventListener('touchend', () => {
        if (Math.abs(distX) > Math.abs(distY) && Math.abs(distX) > threshold) {
          if (distX < 0) {
            handleNext();
          } else {
            handlePrev();
          }
        }
        startX = 0;
        startY = 0;
        distX = 0;
        distY = 0;
      });

      window.addEventListener('resize', () => {
        const checkMobile = window.innerWidth <= 768;
        if (checkMobile !== isMobile) {
          isMobile = checkMobile;
          perView = isMobile ? 1 : 4;
          currentSlide = 0;
          updateDots();
          renderCards();
        }
      });

      updateDots();
      renderCards();
    }

    if (document.readyState === 'loading') {
      document.addEventListener('DOMContentLoaded', initShowcaseSlider);
    } else {
      initShowcaseSlider();
    }
  })();
</script>

{% schema %}
{
  "name": "Animated Showcase",
  "settings": [
    {
      "type": "text",
      "id": "subheading",
      "label": "Subheading",
      "default": "LIMITED OFFERS"
    },
    {
      "type": "text",
      "id": "heading",
      "label": "Heading",
      "default": "FEATURED CURATIONS"
    },
    {
      "type": "text",
      "id": "description",
      "label": "Description",
      "default": "Handpicked essentials at member-exclusive prices"
    }
  ],
  "blocks": [
    {
      "type": "product_item",
      "name": "Product Slide",
      "settings": [
        {
          "type": "product",
          "id": "product",
          "label": "Select Shopify Product"
        },
        {
          "type": "header",
          "content": "Custom / Fallback Overrides"
        },
        {
          "type": "image_picker",
          "id": "custom_image",
          "label": "Custom Product Image"
        },
        {
          "type": "text",
          "id": "custom_title",
          "label": "Custom Title",
          "default": "PREMIUM SELECTION"
        },
        {
          "type": "text",
          "id": "custom_price",
          "label": "Custom Price",
          "default": "₹4,999"
        },
        {
          "type": "text",
          "id": "custom_compare_price",
          "label": "Custom Compare Price",
          "default": "₹7,999"
        }
      ]
    }
  ],
  "presets": [
    {
      "name": "Animated Showcase",
      "blocks": [
        { "type": "product_item" },
        { "type": "product_item" },
        { "type": "product_item" },
        { "type": "product_item" }
      ]
    }
  ]
}
{% endschema %}

```

---

#### Step 3: Add & Customize the Section in Theme Customizer

1. Go to **Online Store** $\rightarrow$ **Themes** and click **Customize**.
2. Navigate to the page where you want the slider displayed (e.g., Homepage).
3. In the left panel, scroll to the bottom of the section list and click **Add section**.
4. Search for or select **Animated Showcase**.
5. Once added, you can:
* **Edit Section Copy:** Change the Subheading badge, Main Title, and Description.
* **Select Products Directly:** Click any nested **Product Slide** block, click **Select product**, and choose any item from your active catalog. Title, pricing, image, and links sync automatically.
* **Add/Remove Products:** Click **Add Product Slide** to include up to 12+ items, or click the trash icon on any block to remove it.
* **Reorder on the Fly:** Drag and drop the handle icons ($\vdots\vdots$) next to any product block to rearrange the display sequence.


6. Click **Save** in the top-right corner.

---

### Verifying Desktop & Mobile Functionality

* **Desktop Verification ($>768\text{px}$):**
* Confirm 4 products sit side-by-side without overflowing horizontal screen bounds.
* Hover over each card to verify it snaps upright (`0°`), elevates slightly, and projects an unclipped 360° golden boundary glow.
* Click the **Next** ($\rightarrow$) and **Prev** ($\leftarrow$) arrows to ensure cards execute the directional slide exit and entrance.


* **Mobile Verification ($\le 768\text{px}$):**
* Open DevTools or test on a mobile device. Exactly one centered product card should render.
* Swipe horizontally across the product card: swiping left advances to the next slide; swiping right moves to the previous slide.
* Swipe vertically down the screen over the product: confirm the page scrolls naturally without misfiring the carousel.
