<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Афиша — Mobile</title>
  <style>
    :root{
      --bg:#2f2f2f;
      --phone:#f5f5f5;
      --black:#111;
      --text:#111;
      --muted:#666;
      --grey:#cfcfcf;
      --grey2:#e7e7e7;
      --chip:#dedede;
      --chip-on:#111;
      --chip-on-text:#fff;
      --blueCard:#dff1fb;
      --red:#ef3b2d;
      --radius:28px;
      --radius-sm:16px;
      --nav-h:78px;
      --shadow:0 10px 30px rgba(0,0,0,.25);
    }
    *{box-sizing:border-box}
    body{
      margin:0; font-family: system-ui, -apple-system, Segoe UI, Roboto, Arial, sans-serif;
      background:var(--bg); height:100vh; display:grid; place-items:center; color:var(--text);
    }

    /* ----- PHONE FRAME FOR DESKTOP PREVIEW ----- */
    .wrap{
      width:min(420px, 100vw);
      height:min(860px, 100vh);
      padding:12px;
    }
    .phone{
      height:100%; width:100%; background:var(--phone); border-radius:40px;
      position:relative; overflow:hidden; box-shadow:var(--shadow);
    }

    /* ----- ICONS ----- */
    .ic{width:20px;height:20px; display:inline-block;}
    .ic.sm{width:18px;height:18px;}
    .ic.lg{width:24px;height:24px;}
    .ic-btn{
      width:34px;height:34px;border-radius:50%;
      display:grid;place-items:center;
      background:#fff; box-shadow:0 2px 6px rgba(0,0,0,.15);
      cursor:pointer; user-select:none;
      transition:.15s transform,.15s background;
    }
    .ic-btn:active{transform:scale(.92); background:#f0f0f0}

    /* ----- TOP BAR ----- */
    .topbar{
      position:sticky; top:0; z-index:5;
      background:transparent; padding:14px 16px 8px;
      display:flex; align-items:center; justify-content:space-between;
    }
    .logo{
      font-weight:800; font-size:12px; color:#fff;
      background:var(--red); padding:4px 8px; border-radius:20px;
      transform:skewX(-8deg);
    }
    .top-actions{display:flex; gap:10px;}
    .back-btn{
      width:34px;height:34px;border-radius:50%; background:#fff; display:grid; place-items:center; cursor:pointer;
      box-shadow:0 2px 6px rgba(0,0,0,.15);
      transition:.15s transform;
    }
    .back-btn:active{transform:scale(.92)}

    /* ----- HEADER MAP BLOCK ----- */
    .map-header{
      margin:0 12px; border-radius:26px; overflow:hidden; position:relative;
      height:150px; background:
        radial-gradient(500px 160px at 60% 0%, rgba(239,59,45,.25), transparent 55%),
        url('https://images.unsplash.com/photo-1548345680-f5475ea5df84?q=80&w=1200&auto=format&fit=crop') center/cover;
      color:#fff;
    }
    .map-header .overlay{
      position:absolute; inset:0;
      background:linear-gradient(180deg, rgba(0,0,0,.55), rgba(0,0,0,.25) 55%, rgba(0,0,0,0));
    }
    .map-header .content{
      position:absolute; inset:0; padding:14px;
      display:flex; flex-direction:column; justify-content:space-between;
    }
    .place-title{
      font-size:20px; font-weight:800;
    }
    .header-row{
      display:flex; align-items:center; justify-content:space-between;
    }
    .header-icons{
      display:flex; gap:8px;
    }
    .header-icons .ic-btn{
      width:30px;height:30px;
      border-radius:50%;
      background:rgba(255,255,255,.95);
      box-shadow:none;
    }

    /* ----- CATEGORY TABS ----- */
    .cats{
      margin:8px 14px 0; display:flex; gap:10px; overflow-x:auto; padding-bottom:6px;
      scrollbar-width:none;
    }
    .cats::-webkit-scrollbar{display:none}
    .cat{
      position:relative; flex:0 0 auto; font-size:14px; color:#333; cursor:pointer;
      padding:6px 10px; user-select:none; border-radius:999px; background:#fff; border:1px solid #e9e9e9;
    }
    .cat.active{color:#fff; font-weight:700; background:#111; border-color:#111;}

    /* ----- PULL SHEET HANDLE ----- */
    .handle{
      width:48px;height:6px;border-radius:999px;background:#d9d9d9; margin:10px auto 6px;
    }

    /* ----- PAGES + TRANSITIONS ----- */
    main{
      position:relative; height:calc(100% - var(--nav-h));
      overflow:hidden;
      padding-bottom:var(--nav-h);
    }
    .page{
      position:absolute; inset:0; overflow:auto; -webkit-overflow-scrolling:touch;
      opacity:0; transform:translateX(100%); pointer-events:none;
      transition:transform .35s cubic-bezier(.2,.8,.2,1), opacity .2s ease;
      padding-bottom:20px;
    }
    .page.active{opacity:1; transform:translateX(0); pointer-events:auto;}
    .page.exit-left{transform:translateX(-20%); opacity:0;}
    .page.exit-right{transform:translateX(20%); opacity:0;}

    /* ----- HOME CONTENT ----- */
    .section{ margin:8px 14px; }
    .carousel{
      display:flex; gap:10px; overflow-x:auto; padding:8px 2px 2px;
      scrollbar-width:none;
    }
    .carousel::-webkit-scrollbar{display:none}
    .mini-card{
      flex:0 0 150px; border-radius:18px; background:#fff; overflow:hidden;
      box-shadow:0 2px 8px rgba(0,0,0,.12);
      cursor:pointer;
      transition:.2s transform;
    }
    .mini-card:active{transform:scale(.97)}
    .mini-img{ height:120px; background:#222 center/cover; }
    .mini-body{
      background:var(--blueCard); padding:8px 10px; height:70px; display:flex; flex-direction:column; justify-content:center;
    }
    .mini-title{font-weight:800; font-size:13px; line-height:1.2;}
    .mini-sub{font-size:12px; color:#444; margin-top:4px;}

    .calendar-strip{
      background:#fff; border-radius:20px; padding:10px; box-shadow:0 2px 6px rgba(0,0,0,.1);
      display:flex; gap:8px; overflow-x:auto;
    }
    .day{
      flex:0 0 auto; width:44px; padding:6px 0; border-radius:12px; text-align:center; font-size:12px; color:#333;
      background:var(--grey2); cursor:pointer; user-select:none;
    }
    .day .num{font-weight:800; font-size:14px; margin-top:2px;}
    .day.active{background:#222; color:#fff;}

    .h2{
      font-weight:900; font-size:20px; margin:12px 2px 8px;
    }
    .rec-card{
      background:#fff; border-radius:20px; overflow:hidden; position:relative;
      box-shadow:0 2px 8px rgba(0,0,0,.12);
      cursor:pointer;
    }
    .rec-img{height:160px; background:#000 center/cover;}
    .rec-body{
      background:var(--blueCard); padding:10px 12px; display:flex; justify-content:space-between; align-items:center;
    }
    .rec-title{font-weight:900;}
    .rec-sub{font-size:12px;color:#333;margin-top:2px;}
    .price-pill{
      background:#111; color:#fff;
      border-radius:999px; padding:6px 10px; font-weight:900; font-size:12px;
    }

    /* ----- MAP PAGE ----- */
    .map-full{
      margin:12px; height:520px; background:
        url('https://images.unsplash.com/photo-1526481280695-3c687fd643ed?q=80&w=1200&auto=format&fit=crop') center/cover;
      border-radius:26px; position:relative; overflow:hidden;
    }
    .pin{
      position:absolute; left:52%; top:45%; transform:translate(-50%,-100%);
      font-size:0;
      width:40px;height:40px; display:grid; place-items:center;
      filter:drop-shadow(0 6px 6px rgba(0,0,0,.4));
    }

    /* ----- DETAIL PAGE ----- */
    .detail{ padding:0 12px; }
    .detail-hero{
      margin:12px 0 0; height:210px; border-radius:22px; overflow:hidden;
      background:#000 center/cover;
      position:relative;
    }
    .detail-title{
      font-weight:900; font-size:20px; text-align:center; margin:12px 0 8px;
    }
    .bubble{
      background:#eaeaea; border-radius:16px; padding:12px; font-size:14px; color:#222; line-height:1.35;
      margin:0 4px 12px;
    }
    .btn-red{
      background:var(--red); color:#fff; border:0; width:100%; padding:14px;
      border-radius:999px; font-weight:900; font-size:16px; cursor:pointer;
      transition:.15s transform;
    }
    .btn-red:active{transform:scale(.98)}
    .row{ display:flex; gap:10px; align-items:center; }
    .btn-black{
      flex:1; background:#111; color:#fff; border:0; padding:12px 14px; border-radius:999px; font-weight:800; cursor:pointer;
    }
    .heart{
      width:52px;height:52px;border-radius:50%; background:#111; color:#fff; display:grid; place-items:center; cursor:pointer;
      transition:.15s transform,.15s background;
    }
    .heart:active{transform:scale(.95)}
    .heart.active{background:var(--red);}
    .reviews{
      margin:12px 2px 6px; font-weight:900; font-size:18px;
    }
    .seg{
      display:flex; gap:10px; margin:6px 2px 0;
    }
    .seg-btn{
      background:#fff; border-radius:10px; padding:8px 12px; font-size:14px; border:1px solid #ddd; cursor:pointer;
    }
    .seg-btn.active{
      background:var(--red); color:#fff; border-color:var(--red);
      display:flex; align-items:center; gap:6px;
    }
    .review-list{
      margin-top:8px; display:grid; gap:8px;
    }
    .review{
      background:#fff; border-radius:14px; padding:10px 12px; font-size:14px; line-height:1.35;
      border:1px solid #eee;
      display:flex; gap:10px; align-items:flex-start;
    }
    .avatar{
      width:38px;height:38px;border-radius:50%; background:#ddd; display:grid; place-items:center;
    }

    /* ----- SESSIONS PAGE ----- */
    .session-wrap{padding:0 12px;}
    .date-row{
      background:#eee; border-radius:16px; padding:10px; margin:8px 0;
    }
    .date-head{
      display:flex; justify-content:space-between; align-items:center; font-weight:800; margin-bottom:8px;
    }
    .times{display:flex; gap:8px; overflow-x:auto;}
    .time-chip{
      flex:0 0 auto; padding:10px 14px; border-radius:999px; background:#dcdcdc; font-weight:700; cursor:pointer; user-select:none;
    }
    .time-chip.active{background:#111;color:#fff;}
    .btn-black-big{
      background:#111; color:#fff; border:0; width:100%; padding:16px;
      border-radius:999px; font-weight:900; font-size:16px; cursor:pointer; margin-top:10px;
    }

    /* ----- FILTERS PAGE ----- */
    .filters{padding:0 14px;}
    .filters h3{margin:12px 0 6px; font-size:18px; font-weight:900;}
    .filter-row{display:flex; gap:8px; flex-wrap:wrap;}
    .filter-chip{
      background:#fff; border:1px solid #e1e1e1; padding:8px 12px; border-radius:12px; font-size:14px; cursor:pointer;
      display:flex; align-items:center; gap:6px;
    }
    .filter-chip.active{background:#111;color:#fff;border-color:#111;}
    .filter-chip .tick{display:none;}
    .filter-chip.active .tick{display:inline-block;}

    .select-ish{
      margin-top:6px; display:flex; gap:8px; align-items:center;
    }
    .select-btn{
      background:#111; color:#fff; padding:8px 12px; border-radius:10px; font-size:14px; font-weight:800; cursor:pointer;
      display:flex; align-items:center; gap:6px;
    }
    .select-btn.ghost{
      background:#fff;color:#111;border:1px solid #ddd;font-weight:700;
    }

    .cal-box{
      margin-top:10px; background:#fff; border-radius:16px; padding:10px; border:1px solid #eee;
    }
    .cal-head{
      display:flex; justify-content:space-between; align-items:center; margin-bottom:8px;
      font-weight:800;
    }
    .cal-nav{display:flex; gap:8px; align-items:center;}
    .cal-nav button{
      width:28px;height:28px;border-radius:8px;border:1px solid #ddd;background:#fff;cursor:pointer;font-weight:900;
    }
    .cal-select{
      border:1px solid #ddd; padding:4px 8px; border-radius:8px; font-size:13px; background:#fff;
    }
    .cal-grid{
      display:grid; grid-template-columns:repeat(7,1fr); gap:6px; font-size:13px;
    }
    .cal-dow{color:#999; text-align:center; font-weight:700; font-size:12px;}
    .cal-day{
      height:34px; border-radius:8px; display:grid; place-items:center; cursor:pointer; background:#f3f3f3;
    }
    .cal-day.off{opacity:.35;cursor:default;}
    .cal-day.sel{background:#111;color:#fff;font-weight:900;}
    .cal-day.range{background:#d8d8d8;font-weight:800;}

    .filters .place-input{
      margin-top:6px; background:#fff; border:1px solid #ddd; border-radius:12px; padding:12px; font-size:14px; color:#999;
    }
    .filters .done{
      margin:12px 0 18px;
    }

    /* ----- DATE LIST PAGE (calendar + events) ----- */
    .date-page{padding:0 14px;}
    .big-cal{
      background:#fff;border-radius:18px;border:1px solid #eee;padding:10px;margin-top:6px;
    }
    .date-title{
      margin:10px 2px; font-size:20px; font-weight:900;
    }
    .date-events{display:grid; gap:10px;}
    .date-event{
      background:#fff;border-radius:18px;overflow:hidden;box-shadow:0 2px 6px rgba(0,0,0,.08);
    }
    .date-event .img{height:150px;background:#000 center/cover;}
    .date-event .body{background:var(--blueCard); padding:10px 12px; display:flex; justify-content:space-between; align-items:center;}
    .date-event .ttl{font-weight:900;}
    .date-event .sub{font-size:12px;color:#333;margin-top:3px;}

    /* ----- MY EVENTS PAGE (with chat join) ----- */
    .my-events{padding:0 14px;}
    .my-events h2{margin:6px 0 6px; font-size:20px; font-weight:900; text-align:center;}
    .my-events .chipbar{display:flex; gap:8px; overflow:auto; padding-bottom:6px;}
    .event-block{margin-top:8px;}
    .event-date{font-weight:900; font-size:18px; margin:10px 2px;}
    .event-card2{
      background:#fff; border-radius:18px; overflow:hidden; margin-bottom:8px;
      box-shadow:0 2px 6px rgba(0,0,0,.08);
    }
    .event-card2 .img{height:150px;background:#000 center/cover;}
    .event-card2 .body{background:var(--blueCard); padding:10px 12px; display:flex; justify-content:space-between; align-items:center;}
    .event-card2 .ttl{font-weight:900;}
    .event-card2 .sub{font-size:12px;color:#333;margin-top:3px;}
    .join-btn{
      margin:8px 10px 12px; width:calc(100% - 20px);
      background:#111; color:#fff; border:0; padding:12px 14px; border-radius:999px; font-weight:900; cursor:pointer;
      transition:.15s transform;
    }
    .join-btn:active{transform:scale(.98)}

    /* ----- CHAT PAGE ----- */
    .chat{
      height:calc(100% - 56px);
      background:
        radial-gradient(700px 220px at 30% 0%, rgba(239,59,45,.08), transparent 60%),
        #f2f2f2;
      padding:10px 12px 80px;
      display:flex; flex-direction:column; gap:8px;
    }
    .msg{
      max-width:80%; padding:10px 12px; border-radius:16px; font-size:14px; line-height:1.35; position:relative;
      background:#fff; border:1px solid #e6e6e6;
    }
    .msg.me{align-self:flex-end; background:#e9e9e9;}
    .msg .time{position:absolute; right:8px; bottom:-16px; font-size:11px; color:#c44;}
    .msg-row{display:flex; gap:6px; align-items:flex-end;}
    .msg-img{
      width:170px;height:110px;border-radius:14px;background:#000 center/cover;border:1px solid #ddd;
    }
    .chat-input{
      position:fixed; left:50%; transform:translateX(-50%);
      bottom:var(--nav-h); width:min(420px,100vw);
      display:flex; gap:8px; align-items:center; padding:10px 12px; background:#fff; border-top:1px solid #eee;
    }
    .chat-input .more{
      width:36px;height:36px;border-radius:50%; background:#f0f0f0; display:grid; place-items:center; cursor:pointer;
      border:1px solid #e0e0e0;
    }
    .chat-input input{
      flex:1; padding:10px 12px; border-radius:999px; border:1px solid #ddd; outline:none; font-size:14px;
    }
    .chat-input button{
      width:44px;height:44px;border-radius:50%; border:0; background:var(--red); color:#fff; cursor:pointer; font-weight:900;
    }

    /* ----- PROFILE/LOYALTY PAGE ----- */
    .profile{padding:0 14px;}
    .profile-card{
      background:#fff;border-radius:16px; padding:12px; display:flex; gap:10px; align-items:center; border:1px solid #eee;
    }
    .profile-card .avatar{
      width:52px;height:52px;border-radius:14px;background:#dcdcdc; display:grid; place-items:center;
    }
    .profile-name{font-weight:900;}
    .profile-sub{font-size:12px;color:#555;}

    .loyalty-head{
      margin-top:10px; display:flex; justify-content:space-between; align-items:center; font-weight:900; font-size:18px;
    }
    .loyalty-bar{
      margin-top:8px; position:relative; height:6px; background:#ddd; border-radius:999px;
    }
    .loyalty-bar .fill{
      position:absolute; left:0; top:0; bottom:0; width:70%; background:var(--red); border-radius:999px;
    }
    .loyalty-bar .dot{
      position:absolute; top:50%; transform:translate(-50%,-50%);
      width:14px;height:14px;border-radius:50%; background:var(--red);
      border:2px solid #fff;
    }
    .loyalty-bar .dot.d1{left:18%}
    .loyalty-bar .dot.d2{left:70%}

    .lvl-list{margin-top:10px; display:grid; gap:10px;}
    .lvl-card{
      background:#fff;border-radius:14px;padding:12px;border:1px solid #eee; position:relative; cursor:pointer;
      box-shadow:0 2px 6px rgba(0,0,0,.05);
    }
    .lvl-card .ttl{font-weight:900;}
    .lvl-card .sub{font-size:12px;color:#666;margin-top:6px;line-height:1.3;}
    .lvl-card.active{outline:2px solid rgba(239,59,45,.35);}
    .lvl-card .star{
      position:absolute; left:12px; top:12px;
      width:18px;height:18px;
    }

    .to-next{
      margin-top:8px; background:#fff;border:1px solid #eee;border-radius:12px;padding:10px; font-size:14px; display:flex; gap:8px; align-items:center;
    }

    .support{
      margin-top:10px; background:#fff;border-radius:16px;border:1px solid #eee;padding:10px;
    }
    .support-item{
      display:flex; gap:10px; align-items:center; padding:10px; border-radius:10px; cursor:pointer;
    }
    .support-item:hover{background:#f6f6f6;}
    .support-item .label{font-size:15px;}
    .support-item + .support-item{border-top:1px solid #f0f0f0;}

    /* ----- LOYALTY SHEETS ----- */
    .sheet{
      position:fixed; inset:0; background:rgba(0,0,0,.45);
      opacity:0; pointer-events:none; transition:.2s; z-index:30;
      display:flex; align-items:center; justify-content:center;
      padding:16px;
    }
    .sheet.open{opacity:1; pointer-events:auto;}
    .sheet-card{
      width:100%; max-width:420px; background:#fff; border-radius:20px; padding:16px;
      transform:translateY(12px) scale(.98); transition:.25s cubic-bezier(.2,.8,.2,1);
      box-shadow:var(--shadow); position:relative;
    }
    .sheet.open .sheet-card{transform:none;}
    .sheet-card h4{margin:0 0 8px; font-size:18px; font-weight:900;}
    .sheet-card p{margin:0; font-size:14px; color:#222; line-height:1.35;}
    .sheet-close{
      position:absolute; right:10px; top:10px; width:32px;height:32px;border-radius:50%; background:#f0f0f0; display:grid; place-items:center; cursor:pointer;
      border:1px solid #e3e3e3;
    }

    /* ----- BOTTOM NAV ----- */
    nav{
      position:absolute; left:0; right:0; bottom:0; height:var(--nav-h);
      background:#fff; border-top:1px solid #e9e9e9;
      display:grid; grid-template-columns:repeat(4,1fr);
      padding-top:6px;
    }
    .nav-btn{
      display:flex; flex-direction:column; align-items:center; justify-content:center; gap:4px;
      font-size:11px; color:#777; cursor:pointer; user-select:none;
      transition:.15s color, .15s transform;
    }
    .nav-btn .ic{font-size:0;}
    .nav-btn.active{color:#000; font-weight:800;}
    .nav-btn:active{transform:scale(.95)}

    /* ----- UTIL ----- */
    .spacer{height:12px;}
  </style>
</head>
<body>
  <div class="wrap">
    <div class="phone">
      <main>
        <!-- HOME -->
        <section class="page active" id="page-home" data-title="Главный">
          <div class="topbar">
            <div></div>
            <div class="logo">МГУ · АЛЬФА</div>
            <div></div>
          </div>

          <div class="map-header">
            <div class="overlay"></div>
            <div class="content">
              <div class="header-row">
                <div class="place-title">Альфа банк</div>
                <div class="header-icons">
                  <div class="ic-btn" data-action="openFilters" title="Фильтры">
                    <svg class="ic sm" viewBox="0 0 24 24" fill="none" stroke="#111" stroke-width="2">
                      <path d="M3 5h18M6 12h12M10 19h4"/>
                    </svg>
                  </div>
                  <div class="ic-btn" data-route="map" title="Карта">
                    <svg class="ic sm" viewBox="0 0 24 24" fill="none" stroke="#111" stroke-width="2">
                      <path d="M12 21s7-6.2 7-11a7 7 0 10-14 0c0 4.8 7 11 7 11z"/>
                      <circle cx="12" cy="10" r="2.5"/>
                    </svg>
                  </div>
                  <div class="ic-btn" data-route="profile" title="Профиль">
                    <svg class="ic sm" viewBox="0 0 24 24" fill="none" stroke="#111" stroke-width="2">
                      <circle cx="12" cy="8" r="4"/>
                      <path d="M4 21c2.3-3.8 13.7-3.8 16 0"/>
                    </svg>
                  </div>
                </div>
              </div>
              <div class="cats" id="cats"></div>
            </div>
          </div>

          <div class="handle"></div>

          <div class="section">
            <div class="carousel" id="hotCarousel"></div>
          </div>

          <div class="section">
            <div class="calendar-strip" id="calendarStrip"></div>
          </div>

          <div class="section">
            <div class="h2">Рекомендации</div>
            <div id="recList"></div>
          </div>
        </section>

        <!-- MAP -->
        <section class="page" id="page-map" data-title="Карта">
          <div class="topbar">
            <div></div>
            <div class="logo">МГУ · АЛЬФА</div>
            <div class="top-actions">
              <div class="ic-btn" data-action="openFilters" title="Фильтры">
                <svg class="ic sm" viewBox="0 0 24 24" fill="none" stroke="#111" stroke-width="2">
                  <path d="M3 5h18M6 12h12M10 19h4"/>
                </svg>
              </div>
              <div class="ic-btn" data-route="profile" title="Профиль">
                <svg class="ic sm" viewBox="0 0 24 24" fill="none" stroke="#111" stroke-width="2">
                  <circle cx="12" cy="8" r="4"/><path d="M4 21c2.3-3.8 13.7-3.8 16 0"/>
                </svg>
              </div>
            </div>
          </div>

          <div class="section" style="margin-top:0;">
            <div style="font-weight:900;font-size:18px;margin:2px 2px 8px;">Альфа банк</div>
            <div class="map-full">
              <div class="pin">
                <svg class="ic lg" viewBox="0 0 24 24" fill="none" stroke="#ef3b2d" stroke-width="2">
                  <path d="M12 21s7-6.2 7-11a7 7 0 10-14 0c0 4.8 7 11 7 11z"/>
                  <circle cx="12" cy="10" r="2.5"/>
                </svg>
              </div>
            </div>
          </div>
        </section>

        <!-- DETAIL -->
        <section class="page" id="page-detail" data-title="Событие">
          <div class="topbar">
            <div class="back-btn" data-action="back" title="Назад">
              <svg class="ic sm" viewBox="0 0 24 24" fill="none" stroke="#111" stroke-width="2">
                <path d="M15 18l-6-6 6-6"/>
              </svg>
            </div>
            <div class="logo">МГУ · АЛЬФА</div>
            <div class="top-actions">
              <div class="ic-btn" data-action="openFilters" title="Фильтры">
                <svg class="ic sm" viewBox="0 0 24 24" fill="none" stroke="#111" stroke-width="2">
                  <path d="M3 5h18M6 12h12M10 19h4"/>
                </svg>
              </div>
              <div class="ic-btn" data-route="profile" title="Профиль">
                <svg class="ic sm" viewBox="0 0 24 24" fill="none" stroke="#111" stroke-width="2">
                  <circle cx="12" cy="8" r="4"/><path d="M4 21c2.3-3.8 13.7-3.8 16 0"/>
                </svg>
              </div>
            </div>
          </div>

          <div class="detail">
            <div class="detail-hero" id="detailHero"></div>

            <div class="detail-title" id="detailTitle">—</div>

            <div class="bubble" id="detailDesc"></div>

            <!-- primary CTA changes based on soldOut -->
            <button class="btn-red" id="detailPrimaryBtn" data-action="detailPrimary">Купить билет</button>

            <div class="row" style="margin-top:10px;">
              <button class="btn-black" data-action="trailer">Смотреть трейлер</button>
              <div class="heart" id="detailFav" title="В избранное">
                <svg class="ic lg" id="detailFavIc" viewBox="0 0 24 24" fill="none" stroke="#fff" stroke-width="2">
                  <path d="M20.8 4.6a5.5 5.5 0 00-7.8 0L12 5.6l-1-1a5.5 5.5 0 00-7.8 7.8l1 1L12 21l7.8-7.6 1-1a5.5 5.5 0 000-7.8z"/>
                </svg>
              </div>
            </div>

            <div class="reviews">Отзывы</div>
            <div class="seg">
              <div class="seg-btn active" id="segAll">
                <span class="tick">✓</span> Все
              </div>
              <div class="seg-btn" id="segCrit">Критики</div>
            </div>

            <div class="review-list" id="reviewList"></div>
          </div>
        </section>

        <!-- SESSIONS -->
        <section class="page" id="page-sessions" data-title="Сеансы">
          <div class="topbar">
            <div class="back-btn" data-action="back">
              <svg class="ic sm" viewBox="0 0 24 24" fill="none" stroke="#111" stroke-width="2">
                <path d="M15 18l-6-6 6-6"/>
              </svg>
            </div>
            <div class="logo">МГУ · АЛЬФА</div>
            <div></div>
          </div>

          <div class="session-wrap">
            <div class="detail-title" id="sessionTitle" style="margin-top:4px;">—</div>
            <div class="detail-hero" id="sessionHero" style="height:190px;"></div>
            <div id="sessionDates"></div>
            <button class="btn-black-big" data-action="buyFromSessions">Купить билет</button>
          </div>
        </section>

        <!-- DATE LIST PAGE (big calendar + events) -->
        <section class="page" id="page-date" data-title="Дата">
          <div class="topbar">
            <div class="back-btn" data-action="back">
              <svg class="ic sm" viewBox="0 0 24 24" fill="none" stroke="#111" stroke-width="2">
                <path d="M15 18l-6-6 6-6"/>
              </svg>
            </div>
            <div class="logo">МГУ · АЛЬФА</div>
            <div></div>
          </div>

          <div class="date-page">
            <div class="cats" id="dateCats"></div>

            <div class="big-cal">
              <div class="cal-head">
                <div class="cal-nav">
                  <button data-action="calPrev">‹</button>
                  <select class="cal-select" id="calMonth"></select>
                  <select class="cal-select" id="calYear"></select>
                  <button data-action="calNext">›</button>
                </div>
              </div>
              <div class="cal-grid" id="bigCalGrid"></div>
            </div>

            <div class="date-title" id="dateTitle"></div>
            <div class="date-events" id="dateEvents"></div>
          </div>
        </section>

        <!-- FILTERS PAGE -->
        <section class="page" id="page-filters" data-title="Фильтры">
          <div class="topbar">
            <div class="back-btn" data-action="back">
              <svg class="ic sm" viewBox="0 0 24 24" fill="none" stroke="#111" stroke-width="2">
                <path d="M15 18l-6-6 6-6"/>
              </svg>
            </div>
            <div style="font-weight:900;">Фильтры</div>
            <div></div>
          </div>

          <div class="filters">
            <h3>Тип события</h3>
            <div class="filter-row" id="filterType"></div>

            <h3>Город</h3>
            <div class="select-ish">
              <div class="select-btn" id="citySelected"><span class="tick">✓</span> Москва</div>
              <div class="select-btn ghost" data-action="pickCity">Выбрать…</div>
            </div>

            <h3>Дата</h3>
            <div class="select-ish">
              <div class="select-btn" id="dateSelected"><span class="tick">✓</span> 25–27 ноября</div>
              <div class="select-btn ghost" data-action="pickDate">Выбрать…</div>
            </div>

            <div class="cal-box">
              <div class="cal-head">
                <div class="cal-nav">
                  <button data-action="calPrev">‹</button>
                  <select class="cal-select" id="fltMonth"></select>
                  <select class="cal-select" id="fltYear"></select>
                  <button data-action="calNext">›</button>
                </div>
              </div>
              <div class="cal-grid" id="fltCalGrid"></div>
            </div>

            <h3>Место</h3>
            <div class="place-input">Выбрать…</div>

            <div class="done">
              <button class="btn-red" data-action="applyFilters">Готово</button>
            </div>
          </div>
        </section>

        <!-- MY EVENTS (with join chat) -->
        <section class="page" id="page-my-events" data-title="Мои события">
          <div class="topbar">
            <div></div>
            <div class="logo">МГУ · АЛЬФА</div>
            <div class="top-actions">
              <div class="ic-btn" data-action="openFilters" title="Фильтры">
                <svg class="ic sm" viewBox="0 0 24 24" fill="none" stroke="#111" stroke-width="2">
                  <path d="M3 5h18M6 12h12M10 19h4"/>
                </svg>
              </div>
              <div class="ic-btn" data-route="profile" title="Профиль">
                <svg class="ic sm" viewBox="0 0 24 24" fill="none" stroke="#111" stroke-width="2">
                  <circle cx="12" cy="8" r="4"/><path d="M4 21c2.3-3.8 13.7-3.8 16 0"/>
                </svg>
              </div>
            </div>
          </div>

          <div class="my-events">
            <h2>Мои события</h2>
            <div class="chipbar" id="myCats"></div>

            <div class="event-block" id="myEventsBlock"></div>
          </div>
        </section>

        <!-- CHAT PAGE -->
        <section class="page" id="page-chat" data-title="Чат">
          <div class="topbar">
            <div class="back-btn" data-action="back">
              <svg class="ic sm" viewBox="0 0 24 24" fill="none" stroke="#111" stroke-width="2">
                <path d="M15 18l-6-6 6-6"/>
              </svg>
            </div>
            <div style="font-weight:900;" id="chatTitle">Чат события</div>
            <div></div>
          </div>

          <div class="chat" id="chatBody"></div>

          <div class="chat-input">
            <div class="more" title="Ещё">
              <svg class="ic" viewBox="0 0 24 24" fill="#777">
                <circle cx="5" cy="12" r="2.2"/><circle cx="12" cy="12" r="2.2"/><circle cx="19" cy="12" r="2.2"/>
              </svg>
            </div>
            <input id="chatInput" placeholder="Сообщение…" />
            <button data-action="sendMsg">
              <svg class="ic" viewBox="0 0 24 24" fill="none" stroke="#fff" stroke-width="2">
                <path d="M22 2L11 13"/><path d="M22 2l-7 20-4-9-9-4 20-7z"/>
              </svg>
            </button>
          </div>
        </section>

        <!-- PROFILE / LOYALTY -->
        <section class="page" id="page-profile" data-title="Профиль">
          <div class="topbar">
            <div class="back-btn" data-action="back">
              <svg class="ic sm" viewBox="0 0 24 24" fill="none" stroke="#111" stroke-width="2">
                <path d="M15 18l-6-6 6-6"/>
              </svg>
            </div>
            <div class="logo">МГУ · АЛЬФА</div>
            <div class="ic-btn" data-action="openSettings" title="Настройки">
              <svg class="ic sm" viewBox="0 0 24 24" fill="none" stroke="#111" stroke-width="2">
                <circle cx="12" cy="12" r="3"/><path d="M19.4 15a1 1 0 00.2 1.1l.1.1a2 2 0 01-2.8 2.8l-.1-.1a1 1 0 00-1.1-.2 1 1 0 00-.6.9V20a2 2 0 01-4 0v-.2a1 1 0 00-.6-.9 1 1 0 00-1.1.2l-.1.1a2 2 0 01-2.8-2.8l.1-.1a1 1 0 00.2-1.1 1 1 0 00-.9-.6H4a2 2 0 010-4h.2a1 1 0 00.9-.6 1 1 0 00-.2-1.1l-.1-.1a2 2 0 012.8-2.8l.1.1a1 1 0 001.1.2 1 1 0 00.6-.9V4a2 2 0 014 0v.2a1 1 0 00.6.9 1 1 0 001.1-.2l.1-.1a2 2 0 012.8 2.8l-.1.1a1 1 0 00-.2 1.1 1 1 0 00.9.6H20a2 2 0 010 4h-.2a1 1 0 00-.9.6z"/>
              </svg>
            </div>
          </div>

          <div class="profile">
            <div class="profile-card">
              <div class="avatar">
                <svg class="ic lg" viewBox="0 0 24 24" fill="none" stroke="#777" stroke-width="2">
                  <circle cx="12" cy="8" r="4"/><path d="M4 22c2.3-3.8 13.7-3.8 16 0"/>
                </svg>
              </div>
              <div>
                <div class="profile-name">Сергей Багратионов</div>
                <div class="profile-sub">Пользователь</div>
              </div>
            </div>

            <div class="loyalty-head">
              <div>Уровень лояльности</div>
              <div style="font-weight:900;">→</div>
            </div>

            <div class="loyalty-bar">
              <div class="fill"></div>
              <div class="dot d1"></div>
              <div class="dot d2"></div>
            </div>

            <div class="lvl-list" id="lvlList"></div>

            <div class="to-next">
              <svg class="ic sm" viewBox="0 0 24 24" fill="none" stroke="#111" stroke-width="2">
                <path d="M17 21v-2a4 4 0 00-4-4H5a4 4 0 00-4 4v2"/>
                <circle cx="9" cy="7" r="4"/>
                <path d="M23 21v-2a4 4 0 00-3-3.87"/>
                <path d="M16 3.13a4 4 0 010 7.75"/>
              </svg>
              <div>До следующего уровня ещё <b>40 000 ₽</b></div>
            </div>

            <div class="support">
              <div class="support-item" data-action="support">
                <svg class="ic" viewBox="0 0 24 24" fill="none" stroke="#111" stroke-width="2">
                  <path d="M22 16.9v-2a4 4 0 00-4-4H6a4 4 0 00-4 4v2"/><circle cx="12" cy="7" r="4"/>
                </svg>
                <div class="label">Поддержка</div>
              </div>
              <div class="support-item" data-action="faq">
                <svg class="ic" viewBox="0 0 24 24" fill="none" stroke="#111" stroke-width="2">
                  <circle cx="12" cy="12" r="10"/><path d="M9.1 9a3 3 0 115.8 1c0 2-3 2-3 4"/><circle cx="12" cy="17" r="1"/>
                </svg>
                <div class="label">Частые вопросы</div>
              </div>
              <div class="support-item" data-action="rules">
                <svg class="ic" viewBox="0 0 24 24" fill="none" stroke="#111" stroke-width="2">
                  <path d="M4 19.5A2.5 2.5 0 016.5 17H20"/><path d="M6.5 2H20v20H6.5A2.5 2.5 0 014 19.5v-15A2.5 2.5 0 016.5 2z"/>
                </svg>
                <div class="label">Правила сервиса</div>
              </div>
              <div class="support-item" data-action="history">
                <svg class="ic" viewBox="0 0 24 24" fill="none" stroke="#111" stroke-width="2">
                  <circle cx="12" cy="12" r="10"/><path d="M12 6v6l4 2"/>
                </svg>
                <div class="label">История чатов</div>
              </div>
            </div>

            <div class="spacer"></div>
          </div>
        </section>
      </main>

      <!-- bottom nav -->
      <nav>
        <div class="nav-btn active" data-route="home" title="Главный">
          <svg class="ic" viewBox="0 0 24 24" fill="none" stroke="#111" stroke-width="2">
            <path d="M3 11l9-8 9 8"/><path d="M5 10v10h14V10"/>
          </svg>
          <div>Главный</div>
        </div>
        <div class="nav-btn" data-route="map" title="Карта">
          <svg class="ic" viewBox="0 0 24 24" fill="none" stroke="#111" stroke-width="2">
            <path d="M12 21s7-6.2 7-11a7 7 0 10-14 0c0 4.8 7 11 7 11z"/>
            <circle cx="12" cy="10" r="2.5"/>
          </svg>
          <div>Карта</div>
        </div>
        <div class="nav-btn" data-route="my-events" title="Мои события">
          <svg class="ic" viewBox="0 0 24 24" fill="none" stroke="#111" stroke-width="2">
            <path d="M20.8 4.6a5.5 5.5 0 00-7.8 0L12 5.6l-1-1a5.5 5.5 0 00-7.8 7.8l1 1L12 21l7.8-7.6 1-1a5.5 5.5 0 000-7.8z"/>
          </svg>
          <div>Мои события</div>
        </div>
        <div class="nav-btn" data-route="profile" title="Профиль">
          <svg class="ic" viewBox="0 0 24 24" fill="none" stroke="#111" stroke-width="2">
            <circle cx="12" cy="8" r="4"/><path d="M4 21c2.3-3.8 13.7-3.8 16 0"/>
          </svg>
          <div>Профиль</div>
        </div>
      </nav>
    </div>
  </div>

  <!-- Trailer modal -->
  <div class="sheet" id="modalTrailer">
    <div class="sheet-card">
      <div class="sheet-close" data-action="closeTrailer">✕</div>
      <h4>Трейлер</h4>
      <p>Демо: сюда можно вставить iframe YouTube или видео-плеер.</p>
    </div>
  </div>

  <!-- Loyalty sheets -->
  <div class="sheet" id="sheetLvl">
    <div class="sheet-card">
      <div class="sheet-close" data-action="closeLvl">✕</div>
      <h4 id="sheetLvlTitle">Уровень</h4>
      <p id="sheetLvlBody">Описание уровня</p>
    </div>
  </div>

<script>
(() => {
  // ---- MOCK DATA ----
  const categories = ["Все", "Кино", "Театр", "Концерты", "Спорт", "Выставки"];

  const events = [
    {
      id:"m1",
      type:"Кино",
      title:"Иллюзия обмана 3",
      dateShort:"26 ноября",
      date:"2025-11-26",
      place:"Ходынский бульв., 4, ТЦ «Авиапарк»",
      price:1000,
      minPrice:500,
      img:"https://images.unsplash.com/photo-1517602302552-471fe67acf66?q=80&w=1200&auto=format&fit=crop",
      soldOut:false,
      desc:"Неожиданный триквел криминальной дилогии про иллюзионистов. «Четыре всадника» возвращаются вместе с молодым поколением магов."
    },
    {
      id:"m2", type:"Кино", title:"Авиатор", dateShort:"26 ноября", date:"2025-11-26",
      place:"Ходынский бульв., 4, ТЦ «Авиапарк»", price:1000, minPrice:1000,
      img:"https://images.unsplash.com/photo-1507924538820-ede94a04019d?q=80&w=1200&auto=format&fit=crop",
      soldOut:false, desc:"История большого чувства и большой цены за мечту."
    },
    {
      id:"m3", type:"Кино", title:"Август", dateShort:"26 ноября", date:"2025-11-26",
      place:"Ходынский бульв., 4, ТЦ «Авиапарк»", price:1000, minPrice:1000,
      img:"https://images.unsplash.com/photo-1485846234645-a62644f84728?q=80&w=1200&auto=format&fit=crop",
      soldOut:false, desc:"Драма о выборе и взрослении."
    },
    {
      id:"t1", type:"Театр", title:"Анна Каренина", dateShort:"23 ноября", date:"2025-11-23",
      place:"Московский театр оперетты", price:1000, minPrice:1000,
      img:"https://images.unsplash.com/photo-1507923959047-1d2509fe0e9b?q=80&w=1200&auto=format&fit=crop",
      soldOut:true,
      desc:"Упс! Билетов не осталось. Подпишитесь на обновления, и узнайте первым, если они появятся."
    },
  ];

  const sessionsById = {
    m1: [
      { date:"26 ноября", times:["10:00","12:30","15:00","17:30","20:00"], minPrice:500 },
      { date:"27 ноября", times:["13:00","15:30","17:30","20:00"], minPrice:1000 },
      { date:"28 ноября", times:["13:00","15:30","17:30","20:00"], minPrice:1000 },
    ],
    m2: [
      { date:"26 ноября", times:["13:00","15:30","17:30","20:00"], minPrice:1000 },
      { date:"27 ноября", times:["13:00","15:30","17:30","20:00"], minPrice:1000 },
      { date:"28 ноября", times:["13:00","15:30","17:30","20:00"], minPrice:1000 },
    ],
    m3: [
      { date:"26 ноября", times:["13:00","15:30","17:30","20:00"], minPrice:1000 },
      { date:"27 ноября", times:["13:00","15:30","17:30","20:00"], minPrice:1000 },
      { date:"28 ноября", times:["13:00","15:30","17:30","20:00"], minPrice:1000 },
    ],
    t1: [
      { date:"23 ноября", times:["19:00"], minPrice:1000 },
    ]
  };

  const reviewsAll = [
    {name:"Арсений Бортников", text:"Фильм был очень крутой всей семьёй не могли оторваться!"},
    {name:"Мария", text:"Супер постановка и актёры."},
  ];
  const reviewsCrit = [
    {name:"Кинокритик", text:"Триквел уверенно держит темп и зрелищность франшизы."},
  ];

  const loyaltyLevels = [
    {
      id:"l1", title:"Первый уровень",
      short:"Накопительная бонусная система",
      desc:"Накопительная бонусная система."
    },
    {
      id:"l2", title:"Второй уровень",
      short:"Полный возврат средств. Бесплатная бронь мероприятий. Повышение категории билетов.",
      desc:"Полный возврат средств. Бесплатная бронь мероприятий. Повышение категории билетов."
    },
    {
      id:"l3", title:"Третий уровень",
      short:"Гарантированные билеты. Закрытые ивенты. Приоритетный доступ.",
      desc:"Гарантированные билеты. Закрытые ивенты. Приоритетный доступ."
    }
  ];

  // ---- STATE ----
  const state = {
    route:"home",
    previous:"home",
    cat:"Все",
    myCat:"Кино",
    dayIndex:0,

    fav:new Set(JSON.parse(localStorage.getItem("fav")||"[]")),
    currentId:null,

    // filters
    filterType:"Все",
    filterCity:"Москва",
    filterRange:[25,27],
    fltMonth:10, // 0-based (Nov)
    fltYear:2025,
    fltSel:[25,27],

    // big calendar
    calMonth:8, // Sep
    calYear:2025,
    calSel:9,

    activeSessionTime:{}, // id -> {dateIndex, time}

    // chat
    chatMessages: [
      {me:false, text:"Крутой фильм", time:"10:10"},
      {me:true, text:"Актёры оч красивые", time:"10:10"},
      {me:false, text:"Как он проделал этот фокус?", time:"11:40"},
      {me:true, text:"Не знаю, но выглядит потрясающе", time:"11:43"},
      {me:false, text:"Откуда этот персонаж, разве он был раньше?", time:"11:45"},
      {me:true, text:"Да, это же Джек Уайлдер", time:"11:45"},
      {me:true, text:"Надеюсь, что Дэниел Атлас появится", time:"11:50"},
      {me:false, text:"Это что за новые всадники?", time:"10:10"},
      {me:false, img:true, url:"https://images.unsplash.com/photo-1614852206836-5a5b4c47e38b?q=80&w=1200&auto=format&fit=crop", time:"11:55"},
    ]
  };

  // ---- ELEMENTS ----
  const pages = {
    home: document.getElementById("page-home"),
    map: document.getElementById("page-map"),
    detail: document.getElementById("page-detail"),
    sessions: document.getElementById("page-sessions"),
    filters: document.getElementById("page-filters"),
    date: document.getElementById("page-date"),
    "my-events": document.getElementById("page-my-events"),
    chat: document.getElementById("page-chat"),
    profile: document.getElementById("page-profile"),
  };

  const catsEl = document.getElementById("cats");
  const hotCarousel = document.getElementById("hotCarousel");
  const calendarStrip = document.getElementById("calendarStrip");
  const recList = document.getElementById("recList");

  const detailHero = document.getElementById("detailHero");
  const detailTitle = document.getElementById("detailTitle");
  const detailDesc = document.getElementById("detailDesc");
  const detailFav = document.getElementById("detailFav");
  const detailFavIc = document.getElementById("detailFavIc");
  const detailPrimaryBtn = document.getElementById("detailPrimaryBtn");
  const reviewList = document.getElementById("reviewList");
  const segAll = document.getElementById("segAll");
  const segCrit = document.getElementById("segCrit");

  const sessionTitle = document.getElementById("sessionTitle");
  const sessionHero = document.getElementById("sessionHero");
  const sessionDates = document.getElementById("sessionDates");

  const filterTypeEl = document.getElementById("filterType");
  const fltMonthSel = document.getElementById("fltMonth");
  const fltYearSel = document.getElementById("fltYear");
  const fltCalGrid = document.getElementById("fltCalGrid");
  const dateSelected = document.getElementById("dateSelected");
  const citySelected = document.getElementById("citySelected");

  const dateCats = document.getElementById("dateCats");
  const calMonthSel = document.getElementById("calMonth");
  const calYearSel = document.getElementById("calYear");
  const bigCalGrid = document.getElementById("bigCalGrid");
  const dateTitle = document.getElementById("dateTitle");
  const dateEvents = document.getElementById("dateEvents");

  const myCats = document.getElementById("myCats");
  const myEventsBlock = document.getElementById("myEventsBlock");

  const chatTitle = document.getElementById("chatTitle");
  const chatBody = document.getElementById("chatBody");
  const chatInput = document.getElementById("chatInput");

  const lvlList = document.getElementById("lvlList");

  const modalTrailer = document.getElementById("modalTrailer");
  const sheetLvl = document.getElementById("sheetLvl");
  const sheetLvlTitle = document.getElementById("sheetLvlTitle");
  const sheetLvlBody = document.getElementById("sheetLvlBody");

  // ---- ROUTING ----
  function setRoute(route, dir="forward"){
    if(state.route === route) return;
    state.previous = state.route;

    const prev = pages[state.route];
    const next = pages[route];

    prev.classList.remove("active");
    prev.classList.add(dir==="forward" ? "exit-left" : "exit-right");
    next.classList.add("active");

    setTimeout(()=>prev.classList.remove("exit-left","exit-right"), 380);
    state.route = route;

    document.querySelectorAll(".nav-btn").forEach(b=>{
      b.classList.toggle("active", b.dataset.route===route);
    });
  }

  function back(){
    const t = state.previous && state.previous !== state.route ? state.previous : "home";
    setRoute(t, "back");
  }

  // ---- RENDER HOME ----
  function renderCats(){
    catsEl.innerHTML = categories.map(c => `
      <div class="cat ${state.cat===c?'active':''}" data-cat="${c}">${c}</div>
    `).join("");
  }

  function renderCarousel(){
    const list = events.filter(e => state.cat==="Все" || e.type===state.cat).slice(0,6);
    hotCarousel.innerHTML = list.map(e => `
      <div class="mini-card" data-action="openDetail" data-id="${e.id}">
        <div class="mini-img" style="background-image:url('${e.img}')"></div>
        <div class="mini-body">
          <div class="mini-title">${e.title}</div>
          <div class="mini-sub">${e.dateShort}</div>
        </div>
      </div>
    `).join("");
  }

  function renderCalendarStrip(){
    const days = [
      {w:"Пн", n:1},{w:"Вт", n:2},{w:"Ср", n:3},{w:"Чт", n:4},
      {w:"Пт", n:5},{w:"Сб", n:6},{w:"Вс", n:7},{w:"Пн", n:8},
    ];
    calendarStrip.innerHTML = days.map((d,i)=>`
      <div class="day ${state.dayIndex===i?'active':''}" data-day="${i}">
        <div>${d.w}</div><div class="num">${d.n}</div>
      </div>
    `).join("");
  }

  function renderRecs(){
    const recs = events.filter(x=>x.type==="Кино").slice(0,2);
    recList.innerHTML = recs.map(e=>`
      <div class="rec-card" data-action="openDetail" data-id="${e.id}">
        <div class="rec-img" style="background-image:url('${e.img}')"></div>
        <div class="rec-body">
          <div>
            <div class="rec-title">${e.title}</div>
            <div class="rec-sub">${e.dateShort}</div>
          </div>
          <div class="price-pill">От ${e.minPrice} ₽</div>
        </div>
      </div>
    `).join("");
  }

  function renderHome(){
    renderCats();
    renderCarousel();
    renderCalendarStrip();
    renderRecs();
  }

  // ---- DETAIL ----
  function renderDetail(id){
    const e = events.find(x=>x.id===id);
    if(!e) return;
    state.currentId = id;

    detailHero.style.backgroundImage = `url('${e.img}')`;
    detailTitle.textContent = e.title;
    detailDesc.textContent = e.desc;

    const isFav = state.fav.has(id);
    detailFav.classList.toggle("active", isFav);
    detailFavIc.setAttribute("fill", isFav ? "#fff" : "none");

    // sold out switch (Анна Каренина -> Подписаться)
    if(e.soldOut){
      detailPrimaryBtn.textContent = "Подписаться";
      detailPrimaryBtn.dataset.mode = "subscribe";
    }else{
      detailPrimaryBtn.textContent = "Купить билет";
      detailPrimaryBtn.dataset.mode = "buy";
    }

    renderReviews("all");
  }

  function renderReviews(mode){
    const data = mode==="crit" ? reviewsCrit : reviewsAll;
    reviewList.innerHTML = data.map(r=>`
      <div class="review">
        <div class="avatar"></div>
        <div>
          <div style="font-weight:900;margin-bottom:4px;">${r.name}</div>
          <div>${r.text}</div>
        </div>
      </div>
    `).join("");
    segAll.classList.toggle("active", mode==="all");
    segCrit.classList.toggle("active", mode==="crit");
  }

  // ---- SESSIONS ----
  function renderSessions(id){
    const e = events.find(x=>x.id===id);
    const ds = sessionsById[id] || [];
    sessionTitle.textContent = e?.title || "Сеансы";
    sessionHero.style.backgroundImage = `url('${e?.img || ""}')`;

    if(!state.activeSessionTime[id]){
      state.activeSessionTime[id] = {dateIndex:0, time:(ds[0]?.times[0] || null)};
    }

    sessionDates.innerHTML = ds.map((d,di)=>{
      const activeDate = state.activeSessionTime[id].dateIndex===di;
      return `
        <div class="date-row">
          <div class="date-head">
            <div>${d.date}</div>
            <div class="price-pill">От ${d.minPrice} ₽</div>
          </div>
          <div class="times">
            ${d.times.map(t=>{
              const activeTime = activeDate && state.activeSessionTime[id].time===t;
              return `<div class="time-chip ${activeTime?'active':''}" data-time="${t}" data-di="${di}">${t}</div>`;
            }).join("")}
          </div>
        </div>
      `;
    }).join("");
  }

  // ---- FILTERS ----
  function renderFilterTypes(){
    const types = ["Все","Кино","Театр","Концерты","Спорт"];
    filterTypeEl.innerHTML = types.map(t=>`
      <div class="filter-chip ${state.filterType===t?'active':''}" data-filter-type="${t}">
        <span class="tick">✓</span> ${t}
      </div>
    `).join("");
  }

  const monthNames = ["Янв","Фев","Мар","Апр","Май","Июн","Июл","Авг","Сен","Окт","Ноя","Дек"];

  function fillMonthYearSelect(monthSel, yearSel){
    monthSel.innerHTML = monthNames.map((m,i)=>`<option value="${i}">${m}</option>`).join("");
    yearSel.innerHTML = [2024,2025,2026].map(y=>`<option value="${y}">${y}</option>`).join("");
  }

  function renderMiniCalendar(gridEl, month, year, rangeSel){
    const first = new Date(year, month, 1);
    const startDow = (first.getDay()+6)%7; // Mon=0
    const daysIn = new Date(year, month+1, 0).getDate();

    gridEl.innerHTML = `
      ${["Su","Mo","Tu","We","Th","Fr","Sa"].map(d=>`<div class="cal-dow">${d}</div>`).join("")}
      ${Array.from({length:startDow}).map(()=>`<div></div>`).join("")}
      ${Array.from({length:daysIn}).map((_,i)=>{
        const day = i+1;
        const [a,b] = rangeSel;
        const isSel = day===a || day===b;
        const isRange = day>a && day<b;
        return `<div class="cal-day ${isSel?'sel':''} ${isRange?'range':''}" data-cal-day="${day}">${day}</div>`;
      }).join("")}
    `;
  }

  function renderFilters(){
    renderFilterTypes();
    fillMonthYearSelect(fltMonthSel, fltYearSel);
    fltMonthSel.value = state.fltMonth;
    fltYearSel.value = state.fltYear;
    renderMiniCalendar(fltCalGrid, state.fltMonth, state.fltYear, state.fltSel);

    citySelected.innerHTML = `<span class="tick">✓</span> ${state.filterCity}`;
    dateSelected.innerHTML = `<span class="tick">✓</span> ${state.fltSel[0]}–${state.fltSel[1]} ноября`;
  }

  // ---- BIG DATE PAGE ----
  function renderDateCats(){
    dateCats.innerHTML = categories.map(c => `
      <div class="cat ${state.cat===c?'active':''}" data-cat="${c}">${c}</div>
    `).join("");
  }

  function renderBigCalendar(){
    fillMonthYearSelect(calMonthSel, calYearSel);
    calMonthSel.value = state.calMonth;
    calYearSel.value = state.calYear;

    const year = state.calYear, month = state.calMonth;
    const first = new Date(year, month, 1);
    const startDow = (first.getDay()+6)%7;
    const daysIn = new Date(year, month+1, 0).getDate();

    bigCalGrid.innerHTML = `
      ${["Su","Mo","Tu","We","Th","Fr","Sa"].map(d=>`<div class="cal-dow">${d}</div>`).join("")}
      ${Array.from({length:startDow}).map(()=>`<div></div>`).join("")}
      ${Array.from({length:daysIn}).map((_,i)=>{
        const day = i+1;
        const sel = day===state.calSel;
        return `<div class="cal-day ${sel?'sel':''}" data-bigcal-day="${day}">${day}</div>`;
      }).join("")}
    `;

    dateTitle.textContent = `${state.calSel} ${monthNames[month].toLowerCase()}`;
    renderDateEvents();
  }

  function renderDateEvents(){
    const month = state.calMonth, year=state.calYear, day=state.calSel;
    const iso = `${year}-${String(month+1).padStart(2,"0")}-${String(day).padStart(2,"0")}`;

    const list = events.filter(e =>
      (state.cat==="Все" || e.type===state.cat) &&
      e.date===iso
    );

    if(!list.length){
      dateEvents.innerHTML = `<div class="bubble">На эту дату событий нет.</div>`;
      return;
    }

    dateEvents.innerHTML = list.map(e=>`
      <div class="date-event" data-action="openDetail" data-id="${e.id}">
        <div class="img" style="background-image:url('${e.img}')"></div>
        <div class="body">
          <div>
            <div class="ttl">${e.title}</div>
            <div class="sub">${(sessionsById[e.id]?.[0]?.times||[]).join(" · ")}</div>
          </div>
          <div class="price-pill">От ${e.minPrice} ₽</div>
        </div>
      </div>
    `).join("");
  }

  // ---- MY EVENTS ----
  function renderMyCats(){
    const types = ["Кино","Театр","Концерты","Спорт","Ещё"];
    myCats.innerHTML = types.map(t=>`
      <div class="cat ${state.myCat===t?'active':''}" data-my-cat="${t}">${t}</div>
    `).join("");
  }

  function renderMyEvents(){
    renderMyCats();

    // for demo: show saved favorites as "my events"
    const list = events.filter(e => state.fav.has(e.id) || e.id==="m1" || e.id==="m2");
    const grouped = {};
    list.forEach(e=>{
      grouped[e.dateShort] = grouped[e.dateShort] || [];
      grouped[e.dateShort].push(e);
    });

    myEventsBlock.innerHTML = Object.entries(grouped).map(([date, arr])=>`
      <div class="event-date">${date}</div>
      ${arr.map(e=>`
        <div class="event-card2">
          <div class="img" style="background-image:url('${e.img}')"></div>
          <div class="body">
            <div>
              <div class="ttl">${e.title}</div>
              <div class="sub">${e.place}</div>
            </div>
            <div class="sub">${date}</div>
          </div>
          <button class="join-btn" data-action="openChat" data-id="${e.id}">
            Присоединиться к чату события
          </button>
        </div>
      `).join("")}
    `).join("");
  }

  // ---- CHAT ----
  function renderChat(id){
    const e = events.find(x=>x.id===id);
    chatTitle.textContent = e ? e.title : "Чат события";
    chatBody.innerHTML = state.chatMessages.map(m=>{
      if(m.img){
        return `
          <div class="msg-row">
            <div class="msg">
              <div class="msg-img" style="background-image:url('${m.url}')"></div>
              <div class="time">${m.time}</div>
            </div>
          </div>
        `;
      }
      return `
        <div class="msg ${m.me?'me':''}">
          ${m.text}
          <div class="time">${m.time}</div>
        </div>
      `;
    }).join("");
    chatBody.scrollTop = chatBody.scrollHeight;
  }

  function sendMsg(){
    const text = chatInput.value.trim();
    if(!text) return;
    const time = new Date().toLocaleTimeString("ru-RU",{hour:"2-digit",minute:"2-digit"});
    state.chatMessages.push({me:true,text,time});
    chatInput.value="";
    renderChat(state.currentId);
  }

  // ---- PROFILE / LOYALTY ----
  function renderLoyalty(){
    lvlList.innerHTML = loyaltyLevels.map((l,idx)=>`
      <div class="lvl-card ${idx===0?'active':''}" data-action="openLvl" data-lvl="${l.id}">
        <svg class="star" viewBox="0 0 24 24" fill="none" stroke="#ef3b2d" stroke-width="2">
          <path d="M12 2l3 7 7 .6-5.3 4.6 1.7 7-6.4-3.8-6.4 3.8 1.7-7L2 9.6 9 9l3-7z"/>
        </svg>
        <div class="ttl">${l.title}</div>
        <div class="sub">${l.short}</div>
      </div>
    `).join("");
  }

  function openLvl(id){
    const l = loyaltyLevels.find(x=>x.id===id);
    if(!l) return;
    sheetLvlTitle.textContent = l.title;
    sheetLvlBody.textContent = l.desc;
    sheetLvl.classList.add("open");
  }

  // ---- FAVORITES ----
  function toggleFav(id){
    if(state.fav.has(id)) state.fav.delete(id);
    else state.fav.add(id);
    localStorage.setItem("fav", JSON.stringify([...state.fav]));
    if(state.route==="detail") renderDetail(id);
    renderMyEvents();
  }

  // ---- INITIAL RENDER ----
  renderHome();
  renderFilters();
  renderDateCats();
  renderBigCalendar();
  renderMyEvents();
  renderLoyalty();

  // ---- EVENTS ----
  document.addEventListener("click",(e)=>{
    // bottom nav
    const nav = e.target.closest(".nav-btn");
    if(nav){
      setRoute(nav.dataset.route);
      return;
    }

    // direct route buttons
    const r = e.target.closest("[data-route]");
    if(r){
      setRoute(r.dataset.route);
      return;
    }

    // category tabs home/date
    const cat = e.target.closest("[data-cat]");
    if(cat){
      state.cat = cat.dataset.cat;
      renderHome();
      renderDateCats();
      renderBigCalendar();
      return;
    }

    // my events tabs
    const mycat = e.target.closest("[data-my-cat]");
    if(mycat){
      state.myCat = mycat.dataset.myCat;
      renderMyEvents();
      return;
    }

    // calendar strip day tap -> open date page
    const day = e.target.closest(".day");
    if(day){
      state.dayIndex = +day.dataset.day;
      renderCalendarStrip();
      setRoute("date");
      return;
    }

    // filter type tap
    const ftype = e.target.closest("[data-filter-type]");
    if(ftype){
      state.filterType = ftype.dataset.filterType;
      renderFilterTypes();
      return;
    }

    // mini-calendar day tap (filters)
    const calDay = e.target.closest("[data-cal-day]");
    if(calDay){
      const d = +calDay.dataset.calDay;
      // simple range toggle: first click sets start, second sets end
      const [a,b] = state.fltSel;
      if(d<=a){ state.fltSel=[d,b]; }
      else if(d>=b){ state.fltSel=[a,d]; }
      else{
        // tap inside -> move closer end
        state.fltSel=[a,d];
      }
      renderFilters();
      return;
    }

    // big calendar day tap
    const bigDay = e.target.closest("[data-bigcal-day]");
    if(bigDay){
      state.calSel = +bigDay.dataset.bigcalDay;
      renderBigCalendar();
      return;
    }

    // month/year select changes
    if(e.target===fltMonthSel || e.target===fltYearSel){
      state.fltMonth = +fltMonthSel.value;
      state.fltYear = +fltYearSel.value;
      renderFilters();
      return;
    }
    if(e.target===calMonthSel || e.target===calYearSel){
      state.calMonth = +calMonthSel.value;
      state.calYear = +calYearSel.value;
      renderBigCalendar();
      return;
    }

    const act = e.target.closest("[data-action]");
    if(act){
      const a = act.dataset.action;
      const id = act.dataset.id;

      switch(a){
        case "openDetail":
          renderDetail(id);
          setRoute("detail");
          break;
        case "back": back(); break;
        case "openFilters":
          renderFilters();
          setRoute("filters");
          break;
        case "applyFilters":
          // apply filterType to category and return home
          state.cat = state.filterType;
          renderHome();
          setRoute("home","back");
          break;
        case "calPrev":
          if(state.route==="filters"){
            state.fltMonth--;
            if(state.fltMonth<0){state.fltMonth=11; state.fltYear--;}
            renderFilters();
          }else{
            state.calMonth--;
            if(state.calMonth<0){state.calMonth=11; state.calYear--;}
            renderBigCalendar();
          }
          break;
        case "calNext":
          if(state.route==="filters"){
            state.fltMonth++;
            if(state.fltMonth>11){state.fltMonth=0; state.fltYear++;}
            renderFilters();
          }else{
            state.calMonth++;
            if(state.calMonth>11){state.calMonth=0; state.calYear++;}
            renderBigCalendar();
          }
          break;
        case "detailPrimary":{
          const ecur = events.find(x=>x.id===state.currentId);
          if(!ecur) break;
          if(ecur.soldOut){
            alert("Вы подписались на обновления (демо).");
          }else{
            renderSessions(state.currentId);
            setRoute("sessions");
          }
          break;
        }
        case "buyFromSessions":
          toggleFav(state.currentId); // демо: покупка добавляет в мои события
          setRoute("my-events");
          break;
        case "trailer":
          modalTrailer.classList.add("open");
          break;
        case "closeTrailer":
          modalTrailer.classList.remove("open");
          break;
        case "openChat":
          state.currentId = id;
          renderChat(id);
          setRoute("chat");
          break;
        case "sendMsg":
          sendMsg();
          break;
        case "openLvl":
          openLvl(act.dataset.lvl);
          break;
        case "closeLvl":
          sheetLvl.classList.remove("open");
          break;
        case "openSettings":
          alert("Настройки (демо).");
          break;
        case "pickCity":
          alert("Выбор города (демо).");
          break;
        case "pickDate":
          alert("Выбор диапазона (демо).");
          break;
        case "support":
        case "faq":
        case "rules":
        case "history":
          alert("Раздел в разработке (демо).");
          break;
      }
      return;
    }

    // time chip selection in sessions
    const chip = e.target.closest(".time-chip");
    if(chip && state.route==="sessions"){
      const di = +chip.dataset.di;
      const t = chip.dataset.time;
      const mid = state.currentId;
      state.activeSessionTime[mid] = {dateIndex:di, time:t};
      renderSessions(mid);
      return;
    }

    // reviews segment
    if(e.target.closest("#segAll")) renderReviews("all");
    if(e.target.closest("#segCrit")) renderReviews("crit");

    // favorite in detail
    if(e.target.closest("#detailFav")){
      toggleFav(state.currentId);
    }
  });

  // close sheets by backdrop
  [modalTrailer, sheetLvl].forEach(sh=>{
    sh.addEventListener("click",(e)=>{
      if(e.target===sh) sh.classList.remove("open");
    });
  });

  // send on enter
  chatInput.addEventListener("keydown",(e)=>{
    if(e.key==="Enter") sendMsg();
  });
})();
</script>
</body>
</html>
