<!DOCTYPE html>
<html lang="fa">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>فورت استور</title>
  <style>
    body {
      font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
      background: #f0f5ff;
      color: #222;
      margin: 0;
      padding: 0;
    }
    header {
      background: #0078d7; /* آبی اپیک */
      padding: 25px 15px;
      text-align: center;
      color: white;
      box-shadow: 0 3px 8px rgba(0,0,0,0.15);
    }
    header h1 {
      margin: 0;
      font-size: 2.4rem;
      font-weight: 700;
      letter-spacing: 2px;
      font-family: 'B Yekan', Tahoma, sans-serif;
    }
    .items {
      display: flex;
      flex-wrap: wrap;
      justify-content: center;
      padding: 25px 15px;
      gap: 20px;
      max-width: 1200px;
      margin: 0 auto;
    }
    .item {
      background: white;
      border-radius: 14px;
      box-shadow: 0 6px 15px rgba(0,0,0,0.1);
      width: 280px;
      padding: 15px;
      text-align: center;
      transition: transform 0.3s ease, box-shadow 0.3s ease;
      cursor: default;
    }
    .item:hover {
      transform: translateY(-6px);
      box-shadow: 0 12px 25px rgba(0,0,0,0.15);
    }
    .item img {
      width: 140px;
      height: 140px;
      object-fit: contain;
      margin: 0 auto 15px auto;
      display: block;
    }
    .item h3 {
      font-size: 1.2rem;
      margin: 10px 0 6px 0;
      color: #0078d7;
      min-height: 45px;
    }
    .price {
      color: #0078d7;
      font-weight: 700;
      font-size: 1.15rem;
      margin-bottom: 15px;
    }
    .buy-btn {
      background: #0078d7;
      color: white;
      border: none;
      padding: 12px 25px;
      border-radius: 8px;
      font-weight: 700;
      font-size: 1rem;
      text-decoration: none;
      display: inline-block;
      box-shadow: 0 4px 12px rgba(0,120,215,0.4);
      transition: background-color 0.25s ease;
    }
    .buy-btn:hover {
      background: #005ea3;
      box-shadow: 0 6px 15px rgba(0,94,163,0.7);
    }
    @media (max-width: 650px) {
      .items {
        flex-direction: column;
        align-items: center;
      }
      .item {
        width: 90%;
      }
    }
  </style>
</head>
<body>
  <header>
    <h1>فورت استور</h1>
  </header>
  <section class="items" id="items-container">
    <p style="text-align:center; width:100%; color: #555;">در حال بارگذاری آیتم‌ها...</p>
  </section>

  <script>
    const whatsappNumber = "09131000049";

    function convertPrice(dollarPrice) {
      const dollarToToman = 55000;
      return dollarPrice * dollarToToman + 20000;
    }

    async function fetchShopItems() {
      const container = document.getElementById("items-container");
      container.innerHTML = "<p style='text-align:center; color:#555;'>در حال بارگذاری آیتم‌ها...</p>";

      try {
        const response = await fetch("https://fortnite-api.com/v2/shop/br");
        const data = await response.json();

        if (data && data.data && data.data.daily) {
          container.innerHTML = "";

          data.data.daily.forEach((item) => {
            const name = item.items[0].name || "بدون نام";
            const image = item.items[0].images.icon || "";
            const priceUsd = item.finalPrice / 100;
            const priceToman = convertPrice(priceUsd);

            const message = encodeURIComponent(`سلام، می‌خوام آیتم ${name} رو بخرم.`);
            const whatsappLink = `https://wa.me/98${whatsappNumber.slice(1)}?text=${message}`;

            const itemDiv = document.createElement("div");
            itemDiv.className = "item";

            itemDiv.innerHTML = `
              <img src="${image}" alt="${name}" />
              <h3>${name}</h3>
              <p class="price">${priceToman.toLocaleString()} تومان</p>
              <a href="${whatsappLink}" target="_blank" class="buy-btn">خرید از واتساپ</a>
            `;

            container.appendChild(itemDiv);
          });
        } else {
          container.innerHTML = "<p style='text-align:center; color:#777;'>خطا در دریافت آیتم‌ها</p>";
        }
      } catch (error) {
        container.innerHTML = "<p style='text-align:center; color:#777;'>خطا در بارگذاری داده‌ها</p>";
        console.error(error);
      }
    }

    fetchShopItems();
  </script>
</body>
</html>
