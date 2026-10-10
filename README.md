import React, { useEffect, useRef, useState } from "react";
import {
  Menu,
  X,
  Radio,
  Repeat,
  BarChart3,
  Bell,
  PlayCircle,
  Target,
  Star,
  Check,
  ChevronDown,
  ArrowRight,
  Github,
  Twitter,
  Linkedin,
  Instagram,
  Mail,
  Flame,
} from "lucide-react";

/* ------------------------------------------------------------------ */
/*  STYLES  (keyframes + design tokens + component classes)            */
/*  Tailwind отвечает за раскладку, цвета и эффекты — здесь, в CSS.    */
/* ------------------------------------------------------------------ */
const css = `
@import url('https://fonts.googleapis.com/css2?family=Barlow+Condensed:ital,wght@0,700;0,800;1,800&family=Manrope:wght@400;500;600;700&display=swap');

html { scroll-behavior: smooth; }

.fb-root {
  --bg-primary: #06090a;
  --accent-from: #00e58b;
  --accent-to: #d8f53b;
  --card-bg: rgba(255,255,255,0.045);
  --border: rgba(255,255,255,0.12);
  --text: #eef4f0;
  --muted: #8d9d95;
  --live: #ff4d5e;
  background: var(--bg-primary);
  color: var(--text);
  font-family: 'Manrope', system-ui, -apple-system, 'Segoe UI', sans-serif;
  overflow-x: hidden;
  position: relative;
}
.fb-root *, .fb-root *::before, .fb-root *::after { box-sizing: border-box; }
.fb-root a { color: inherit; text-decoration: none; }
.fb-root button { font-family: inherit; cursor: pointer; }
.fb-root a:focus-visible, .fb-root button:focus-visible, .fb-root input:focus-visible {
  outline: 2px solid var(--accent-from); outline-offset: 3px;
}

.font-display { font-family: 'Barlow Condensed', 'Arial Narrow', Impact, sans-serif; }
.muted { color: var(--muted); }

/* ---------- Keyframes ---------- */
@keyframes fadeInUp { from { opacity: 0; transform: translateY(36px); } to { opacity: 1; transform: translateY(0); } }
@keyframes floatBlob {
  0%   { transform: translate3d(0,0,0) scale(1); }
  50%  { transform: translate3d(40px,-50px,0) scale(1.12); }
  100% { transform: translate3d(-30px,30px,0) scale(0.95); }
}
@keyframes floatY { 0%,100% { transform: translateY(0); } 50% { transform: translateY(-12px); } }
@keyframes livePulse {
  0%   { box-shadow: 0 0 0 0 rgba(255,77,94,.6); }
  70%  { box-shadow: 0 0 0 10px rgba(255,77,94,0); }
  100% { box-shadow: 0 0 0 0 rgba(255,77,94,0); }
}
@keyframes marquee { from { transform: translateX(0); } to { transform: translateX(-50%); } }
@keyframes glowPulse { 0%,100% { opacity: .55; } 50% { opacity: .95; } }
@keyframes spinSlow { to { transform: rotate(360deg); } }

.fiu { animation: fadeInUp .9s cubic-bezier(.2,.8,.2,1) both; }
.d1 { animation-delay: .1s; } .d2 { animation-delay: .25s; } .d3 { animation-delay: .4s; }
.d4 { animation-delay: .55s; } .d5 { animation-delay: .7s; }

/* ---------- Surfaces ---------- */
.glass {
  background: var(--card-bg);
  border: 1px solid var(--border);
  backdrop-filter: blur(18px);
  -webkit-backdrop-filter: blur(18px);
  border-radius: 24px;
}
.grad-text {
  background: linear-gradient(100deg, var(--accent-from), var(--accent-to));
  -webkit-background-clip: text; background-clip: text;
  color: transparent; -webkit-text-fill-color: transparent;
}
.grad-bg { background: linear-gradient(120deg, var(--accent-from), var(--accent-to)); }
.divider {
  height: 1px;
  background: linear-gradient(90deg, transparent, var(--accent-from), var(--accent-to), transparent);
  opacity: .7;
}

.card-hover {
  transition: transform .4s cubic-bezier(.2,.8,.2,1), box-shadow .4s, border-color .4s;
  will-change: transform;
}
.card-hover:hover {
  transform: scale(1.03) translateY(-4px);
  border-color: rgba(0,229,139,.6);
  box-shadow: 0 0 0 1px rgba(0,229,139,.2), 0 24px 70px -18px rgba(0,229,139,.45);
}

/* ---------- Buttons ---------- */
.btn {
  display: inline-flex; align-items: center; justify-content: center; gap: .6rem;
  min-height: 48px; padding: 0 1.75rem; border-radius: 999px;
  font-weight: 700; font-size: .95rem; border: 1px solid transparent;
  transition: transform .3s cubic-bezier(.2,.8,.2,1), box-shadow .3s, background .3s, border-color .3s;
  white-space: nowrap;
}
.btn:hover { transform: scale(1.05); }
.btn-grad {
  color: #04140c;
  background: linear-gradient(120deg, var(--accent-from), var(--accent-to));
  box-shadow: 0 10px 30px -10px rgba(0,229,139,.6);
}
.btn-grad:hover { box-shadow: 0 0 0 4px rgba(0,229,139,.18), 0 14px 48px -6px rgba(0,229,139,.85); }
.btn-ghost {
  color: var(--text); background: rgba(255,255,255,.03); border-color: var(--border);
}
.btn-ghost:hover { background: rgba(255,255,255,.08); border-color: rgba(255,255,255,.35); box-shadow: 0 0 36px -6px rgba(255,255,255,.25); }
.btn-dark { color: #fff; background: #04140c; }
.btn-dark:hover { box-shadow: 0 0 0 4px rgba(4,20,12,.25), 0 14px 40px -8px rgba(0,0,0,.6); }

/* ---------- Hero ---------- */
.h-mega {
  font-family: 'Barlow Condensed', 'Arial Narrow', Impact, sans-serif;
  font-weight: 800; font-style: italic; text-transform: uppercase;
  font-size: clamp(3.6rem, 12vw, 8.5rem);
  line-height: .86; letter-spacing: -0.01em;
}
.h-section {
  font-family: 'Barlow Condensed', 'Arial Narrow', Impact, sans-serif;
  font-weight: 800; text-transform: uppercase;
  font-size: clamp(2.4rem, 6vw, 4.4rem);
  line-height: .95; letter-spacing: -0.005em;
}
.blob {
  position: absolute; border-radius: 9999px; filter: blur(90px);
  animation: floatBlob 14s ease-in-out infinite alternate;
  pointer-events: none;
}
.pitch-lines { position: absolute; inset: 0; width: 100%; height: 100%; opacity: .07; pointer-events: none; }
.hero-mask {
  -webkit-mask-image: radial-gradient(ellipse at 60% 40%, #000 20%, transparent 75%);
  mask-image: radial-gradient(ellipse at 60% 40%, #000 20%, transparent 75%);
}

.live-dot { width: 8px; height: 8px; border-radius: 999px; background: var(--live); animation: livePulse 1.6s infinite; display: inline-block; }
.chip-float { animation: floatY 6s ease-in-out infinite; }

.marquee-track { display: flex; width: max-content; animation: marquee 38s linear infinite; }
.marquee-wrap:hover .marquee-track { animation-play-state: paused; }

/* ---------- Navbar ---------- */
.nav { transition: background .4s, backdrop-filter .4s, border-color .4s, padding .4s; border-bottom: 1px solid transparent; }
.nav.scrolled {
  background: rgba(6,9,10,.72);
  backdrop-filter: blur(18px); -webkit-backdrop-filter: blur(18px);
  border-bottom-color: var(--border);
}
.nav-link { position: relative; color: var(--muted); font-weight: 600; font-size: .92rem; transition: color .25s; padding: .5rem 0; }
.nav-link::after {
  content: ""; position: absolute; left: 0; bottom: 0; height: 2px; width: 100%;
  background: linear-gradient(90deg, var(--accent-from), var(--accent-to));
  transform: scaleX(0); transform-origin: left; transition: transform .3s;
}
.nav-link:hover { color: #fff; }
.nav-link:hover::after { transform: scaleX(1); }

.drawer-overlay { position: fixed; inset: 0; background: rgba(0,0,0,.6); backdrop-filter: blur(4px); transition: opacity .4s; z-index: 60; }
.drawer {
  position: fixed; top: 0; right: 0; bottom: 0; width: 82%; max-width: 360px; z-index: 70;
  background: rgba(10,14,13,.96); border-left: 1px solid var(--border);
  backdrop-filter: blur(24px); -webkit-backdrop-filter: blur(24px);
  transition: transform .5s cubic-bezier(.2,.8,.2,1);
  padding: 1.5rem; display: flex; flex-direction: column;
}

/* ---------- Pricing ---------- */
.toggle-wrap { position: relative; display: inline-flex; padding: 5px; border-radius: 999px; background: var(--card-bg); border: 1px solid var(--border); }
.toggle-pill { position: absolute; top: 5px; bottom: 5px; border-radius: 999px; transition: left .4s cubic-bezier(.2,.8,.2,1), width .4s cubic-bezier(.2,.8,.2,1); }
.toggle-btn { position: relative; z-index: 1; min-height: 44px; padding: 0 1.4rem; background: transparent; border: 0; font-weight: 700; font-size: .92rem; transition: color .3s; display: inline-flex; align-items: center; gap: .5rem; }
.price-pop { border-color: rgba(0,229,139,.55) !important; box-shadow: 0 0 80px -20px rgba(0,229,139,.5); }
@media (min-width: 1024px) {
  .price-pop { transform: scale(1.05); }
  .price-pop:hover { transform: scale(1.08) translateY(-4px); }
}

/* ---------- FAQ ---------- */
.faq-body { display: grid; transition: grid-template-rows .45s cubic-bezier(.2,.8,.2,1); }
.faq-item { transition: border-color .3s, background .3s; border-radius: 20px; }
.faq-item.open { border-color: rgba(0,229,139,.45); background: rgba(0,229,139,.05); }

/* ---------- Inputs ---------- */
.email-input {
  flex: 1; min-width: 0; min-height: 52px; padding: 0 1.25rem; border-radius: 999px;
  background: rgba(4,20,12,.12); border: 1px solid rgba(4,20,12,.35);
  color: #04140c; font-size: 1rem; font-family: inherit;
}
.email-input::placeholder { color: rgba(4,20,12,.6); }

.scroll-x { scroll-snap-type: x mandatory; -webkit-overflow-scrolling: touch; scrollbar-width: none; }
.scroll-x::-webkit-scrollbar { display: none; }

.social { width: 44px; height: 44px; border-radius: 999px; display: inline-flex; align-items: center; justify-content: center; border: 1px solid var(--border); background: var(--card-bg); transition: transform .3s, box-shadow .3s, border-color .3s, color .3s; color: var(--muted); }
.social:hover { transform: scale(1.1) translateY(-2px); color: #fff; border-color: rgba(0,229,139,.6); box-shadow: 0 0 28px -4px rgba(0,229,139,.6); }
.foot-link { color: var(--muted); font-size: .92rem; transition: color .25s; display: inline-block; padding: .35rem 0; }
.foot-link:hover { color: var(--accent-from); }

@media (prefers-reduced-motion: reduce) {
  html { scroll-behavior: auto; }
  .fb-root *, .fb-root *::before, .fb-root *::after { animation: none !important; transition-duration: .01ms !important; }
}
`;

/* ------------------------------------------------------------------ */
/*  HOOKS & SMALL COMPONENTS                                           */
/* ------------------------------------------------------------------ */
function useInView(threshold = 0.2) {
  const ref = useRef(null);
  const [inView, setInView] = useState(false);
  useEffect(() => {
    const el = ref.current;
    if (!el) return;
    if (typeof IntersectionObserver === "undefined") {
      setInView(true);
      return;
    }
    const io = new IntersectionObserver(
      ([entry]) => {
        if (entry.isIntersecting) {
          setInView(true);
          io.disconnect();
        }
      },
      { threshold }
    );
    io.observe(el);
    return () => io.disconnect();
  }, [threshold]);
  return [ref, inView];
}

function Reveal({ children, delay = 0, className = "", style = {} }) {
  const [ref, inView] = useInView(0.15);
  return (
    <div
      ref={ref}
      className={className}
      style={{
        opacity: inView ? 1 : 0,
        transform: inView ? "translateY(0)" : "translateY(36px)",
        transition: `opacity .9s ease ${delay}ms, transform .9s cubic-bezier(.2,.8,.2,1) ${delay}ms`,
        ...style,
      }}
    >
      {children}
    </div>
  );
}

function Counter({ value, suffix = "", decimals = 0 }) {
  const [ref, inView] = useInView(0.4);
  const [n, setN] = useState(0);
  useEffect(() => {
    if (!inView) return;
    const duration = 2000;
    const start = Date.now();
    const id = setInterval(() => {
      const p = Math.min((Date.now() - start) / duration, 1);
      const eased = 1 - Math.pow(1 - p, 3);
      setN(value * eased);
      if (p >= 1) clearInterval(id);
    }, 16);
    return () => clearInterval(id);
  }, [inView, value]);
  return (
    <span ref={ref} className="tabular-nums">
      {n.toFixed(decimals)}
      {suffix}
    </span>
  );
}

function SectionHead({ title, sub, align = "center" }) {
  return (
    <Reveal
      className={align === "center" ? "text-center mx-auto" : ""}
      style={{ maxWidth: 720, marginBottom: "4rem" }}
    >
      <h2 className="h-section">{title}</h2>
      {sub && (
        <p className="muted text-base md:text-lg mt-5" style={{ lineHeight: 1.65 }}>
          {sub}
        </p>
      )}
    </Reveal>
  );
}

function Avatar({ initials, from, to, size = 52 }) {
  return (
    <div
      className="flex items-center justify-center font-bold"
      style={{
        width: size,
        height: size,
        borderRadius: 999,
        background: `linear-gradient(135deg, ${from}, ${to})`,
        color: "#06120c",
        fontSize: size * 0.34,
        flexShrink: 0,
      }}
    >
      {initials}
    </div>
  );
}

/* ------------------------------------------------------------------ */
/*  DATA                                                               */
/* ------------------------------------------------------------------ */
const NAV_LINKS = [
  { label: "Возможности", href: "#features" },
  { label: "Как это работает", href: "#how" },
  { label: "Отзывы", href: "#reviews" },
  { label: "Тарифы", href: "#pricing" },
  { label: "FAQ", href: "#faq" },
];

const FEATURES = [
  { icon: Radio, title: "Live-центр", text: "Счёт, составы и события всех лиг в одной ленте. Обновление без перезагрузки." },
  { icon: Repeat, title: "Радар трансферов", text: "Инсайды с рейтингом достоверности: видно, кто источник и сколько раз он был прав." },
  { icon: BarChart3, title: "Аналитика матча", text: "xG, карты пасов и тепловые зоны — разбор сразу после финального свистка." },
  { icon: Bell, title: "Мгновенные уведомления", text: "Гол любимой команды приходит на телефон раньше, чем успеет крикнуть сосед." },
  { icon: PlayCircle, title: "Голы за 60 секунд", text: "Короткие видеообзоры туров. Все голы, спасения и спорные эпизоды без воды." },
  { icon: Target, title: "Лента под вас", text: "Выберите клубы и лиги — остальное мы уберём. Ни одного лишнего заголовка." },
];

const STATS = [
  { value: 4.2, decimals: 1, suffix: "M", label: "читателей в месяц" },
  { value: 120, decimals: 0, suffix: "K", label: "push-уведомлений в день" },
  { value: 98, decimals: 0, suffix: "%", label: "подтверждённых инсайдов" },
  { value: 850, decimals: 0, suffix: "+", label: "матчей в месяц в live" },
];

const STEPS = [
  { icon: Target, title: "Выберите клубы", text: "Отметьте любимые команды, лиги и игроков. Это занимает меньше минуты." },
  { icon: Bell, title: "Настройте сигналы", text: "Голы, составы, трансферы или только итоги — решаете вы, что и когда приходит." },
  { icon: Flame, title: "Узнавайте первыми", text: "Новость попадает в ленту и на экран телефона, пока остальные ещё ищут её в поиске." },
];

const REVIEWS = [
  {
    name: "Артём Воронин",
    role: "Редактор спортивного отдела",
    initials: "АВ",
    from: "#00e58b",
    to: "#d8f53b",
    text: "Радар трансферов с рейтингом источников сэкономил мне часы проверок. Теперь сначала смотрю, кто сообщил, и только потом решаю, пускать ли в эфир.",
  },
  {
    name: "Мария Левина",
    role: "Аналитик футбольного клуба",
    initials: "МЛ",
    from: "#4de1ff",
    to: "#00e58b",
    text: "Карты пасов и xG появляются через минуты после матча. Для разбора с тренерским штабом этого достаточно, дальше я иду в свои таблицы.",
  },
  {
    name: "Денис Корнеев",
    role: "Болельщик, 11 лет на трибуне",
    initials: "ДК",
    from: "#ffb347",
    to: "#ff5e7a",
    text: "Пропустил гол на 90-й и узнал о нём из пуша за три секунды до того, как заорал весь бар. Ради этого и подписался.",
  },
];

const PLANS = [
  {
    name: "Фанат",
    desc: "Для тех, кто просто любит футбол",
    monthly: 0,
    features: ["Лента по любимым клубам", "Live-счёт всех лиг", "До 5 push в день", "Обзоры туров"],
    cta: "Начать бесплатно",
  },
  {
    name: "Про",
    desc: "Для тех, кто живёт матчами",
    monthly: 390,
    popular: true,
    features: [
      "Всё из тарифа «Фанат»",
      "Радар трансферов",
      "xG, карты пасов, тепловые зоны",
      "Безлимитные push",
      "Видео в 4K без рекламы",
    ],
    cta: "Попробовать 14 дней",
  },
  {
    name: "Клуб и медиа",
    desc: "Для редакций, клубов и подкастов",
    monthly: 2990,
    features: [
      "Всё из тарифа «Про»",
      "API с live-данными",
      "Виджеты под ваш бренд",
      "До 25 рабочих мест",
      "Персональный менеджер",
    ],
    cta: "Связаться с нами",
  },
];

const FAQ = [
  { q: "Как быстро приходят уведомления о голах?", a: "В среднем через 15 секунд после события. Задержка зависит от поставщика данных конкретной лиги — в топ-лигах она минимальна." },
  { q: "Откуда берутся инсайды по трансферам?", a: "Мы собираем сообщения журналистов и клубов, а каждому источнику присваиваем рейтинг по истории точности. Слухи без подтверждения помечаются отдельно." },
  { q: "Можно ли отменить подписку в любой момент?", a: "Да. Отмена в два клика в настройках профиля. Доступ сохраняется до конца оплаченного периода." },
  { q: "Какие лиги и турниры вы покрываете?", a: "Более 60 соревнований: национальные чемпионаты, еврокубки, сборные и молодёжные турниры. Список растёт каждый месяц." },
  { q: "Есть ли мобильное приложение?", a: "Приложения для iOS и Android работают вместе с сайтом: лента, уведомления и избранные клубы синхронизируются между устройствами." },
  { q: "Как работает скидка при оплате за год?", a: "Годовой тариф стоит на 20% дешевле, чем 12 отдельных месяцев. Платёж списывается один раз в год." },
];

const FOOTER_GROUPS = [
  { title: "Продукт", links: ["Live-центр", "Радар трансферов", "Аналитика", "Приложения"] },
  { title: "Лиги", links: ["Премьер-лига", "Лига чемпионов", "Сборные", "Молодёжные"] },
  { title: "Компания", links: ["О нас", "Редакция", "Карьера", "Контакты"] },
  { title: "Поддержка", links: ["Помощь", "API для медиа", "Конфиденциальность", "Условия"] },
];

const TICKER = [
  "Балтика 2:1 Сьерра",
  "Динамит 0:0 Рубикон",
  "Аврора 3:2 Норд",
  "Таганка 1:1 Эльбрус",
  "Мирра 4:0 Каскад",
  "Вектор 1:2 Атлант",
  "Заря 2:2 Прибой",
];

/* ------------------------------------------------------------------ */
/*  SECTIONS                                                           */
/* ------------------------------------------------------------------ */
function Navbar() {
  const [scrolled, setScrolled] = useState(false);
  const [open, setOpen] = useState(false);

  useEffect(() => {
    const onScroll = () => setScrolled(window.scrollY > 50);
    onScroll();
    window.addEventListener("scroll", onScroll, { passive: true });
    return () => window.removeEventListener("scroll", onScroll);
  }, []);

  return (
    <>
      <nav
        className={`nav ${scrolled ? "scrolled" : ""}`}
        style={{ position: "fixed", top: 0, left: 0, right: 0, zIndex: 50 }}
      >
        <div
          className="flex items-center justify-between mx-auto px-6"
          style={{ maxWidth: 1200, height: scrolled ? 68 : 84, transition: "height .4s" }}
        >
          <a href="#top" className="flex items-center gap-3" aria-label="Свисток — на главную">
            <span
              className="grad-bg flex items-center justify-center"
              style={{ width: 40, height: 40, borderRadius: 14, color: "#04140c" }}
            >
              <Radio size={22} strokeWidth={2.4} />
            </span>
            <span className="font-display text-2xl font-bold" style={{ letterSpacing: ".02em" }}>
              СВИСТОК
            </span>
          </a>

          <div className="hidden lg:flex items-center gap-9">
            {NAV_LINKS.map((l) => (
              <a key={l.href} href={l.href} className="nav-link">
                {l.label}
              </a>
            ))}
          </div>

          <div className="flex items-center gap-3">
            <a href="#cta" className="btn btn-grad hidden md:inline-flex">
              Читать бесплатно
            </a>
            <button
              className="lg:hidden flex items-center justify-center"
              onClick={() => setOpen(true)}
              aria-label="Открыть меню"
              style={{
                width: 48, height: 48, borderRadius: 14,
                background: "var(--card-bg)", border: "1px solid var(--border)", color: "#fff",
              }}
            >
              <Menu size={22} />
            </button>
          </div>
        </div>
      </nav>

      {/* Mobile drawer */}
      <div
        className="drawer-overlay"
        onClick={() => setOpen(false)}
        style={{ opacity: open ? 1 : 0, pointerEvents: open ? "auto" : "none" }}
      />
      <aside className="drawer" style={{ transform: open ? "translateX(0)" : "translateX(105%)" }} aria-hidden={!open}>
        <div className="flex items-center justify-between mb-8">
          <span className="font-display text-2xl font-bold">СВИСТОК</span>
          <button
            onClick={() => setOpen(false)}
            aria-label="Закрыть меню"
            className="flex items-center justify-center"
            style={{ width: 48, height: 48, borderRadius: 14, background: "var(--card-bg)", border: "1px solid var(--border)", color: "#fff" }}
          >
            <X size={22} />
          </button>
        </div>
        <div className="flex flex-col">
          {NAV_LINKS.map((l, i) => (
            <a
              key={l.href}
              href={l.href}
              onClick={() => setOpen(false)}
              className="font-display text-3xl font-bold flex items-center justify-between"
              style={{
                padding: "1rem 0", minHeight: 56, borderBottom: "1px solid var(--border)",
                opacity: open ? 1 : 0,
                transform: open ? "translateX(0)" : "translateX(24px)",
                transition: `opacity .5s ease ${150 + i * 70}ms, transform .5s ease ${150 + i * 70}ms`,
              }}
            >
              {l.label}
              <ArrowRight size={20} className="muted" />
            </a>
          ))}
        </div>
        <a href="#cta" onClick={() => setOpen(false)} className="btn btn-grad mt-auto" style={{ width: "100%" }}>
          Читать бесплатно
        </a>
      </aside>
    </>
  );
}

function LiveCard() {
  const [minute, setMinute] = useState(67);
  useEffect(() => {
    const id = setInterval(() => setMinute((m) => (m >= 90 ? 60 : m + 1)), 2500);
    return () => clearInterval(id);
  }, []);

  const events = [
    { m: "12'", t: "Гол — Ковалёв", c: "#00e58b" },
    { m: "38'", t: "Гол — Морено", c: "#4de1ff" },
    { m: "54'", t: "Жёлтая — Ибрагимов", c: "#ffd23f" },
    { m: "63'", t: "Гол — Орлов", c: "#00e58b" },
  ];

  return (
    <div className="relative" style={{ maxWidth: 460, width: "100%", margin: "0 auto" }}>
      {/* floating chips */}
      <div
        className="glass chip-float absolute flex items-center gap-3 px-4 py-3"
        style={{ top: -26, left: -14, borderRadius: 18, zIndex: 3, animationDelay: ".5s" }}
      >
        <span className="grad-bg flex items-center justify-center" style={{ width: 32, height: 32, borderRadius: 10, color: "#04140c" }}>
          <Repeat size={16} />
        </span>
        <div>
          <div className="text-xs muted">Трансфер</div>
          <div className="text-sm font-bold">Морено — подтверждено</div>
        </div>
      </div>
      <div
        className="glass chip-float absolute flex items-center gap-3 px-4 py-3"
        style={{ bottom: -24, right: -10, borderRadius: 18, zIndex: 3, animationDelay: "2s" }}
      >
        <span className="flex items-center justify-center" style={{ width: 32, height: 32, borderRadius: 10, background: "rgba(255,77,94,.18)", color: "var(--live)" }}>
          <Bell size={16} />
        </span>
        <div>
          <div className="text-xs muted">Push · 15 сек</div>
          <div className="text-sm font-bold">Гол! Балтика 2:1</div>
        </div>
      </div>

      <div className="glass p-6 md:p-7" style={{ boxShadow: "0 40px 100px -30px rgba(0,229,139,.35)" }}>
        <div className="flex items-center justify-between mb-6">
          <span className="flex items-center gap-2 text-sm font-bold" style={{ color: "var(--live)" }}>
            <span className="live-dot" /> LIVE {minute}'
          </span>
          <span className="text-sm muted">Лига Севера · 24 тур</span>
        </div>

        <div className="flex items-center justify-between">
          <div className="flex flex-col items-center gap-3" style={{ width: 96 }}>
            <Avatar initials="БФ" from="#00e58b" to="#d8f53b" size={60} />
            <span className="font-bold text-sm">Балтика</span>
          </div>
          <div className="font-display font-extrabold tabular-nums" style={{ fontSize: "4rem", lineHeight: 1 }}>
            2<span className="muted" style={{ margin: "0 .5rem" }}>:</span>1
          </div>
          <div className="flex flex-col items-center gap-3" style={{ width: 96 }}>
            <Avatar initials="СЬ" from="#4de1ff" to="#7b6bff" size={60} />
            <span className="font-bold text-sm">Сьерра</span>
          </div>
        </div>

        {/* progress */}
        <div className="mt-6" style={{ height: 6, borderRadius: 99, background: "rgba(255,255,255,.08)", overflow: "hidden" }}>
          <div className="grad-bg" style={{ height: "100%", width: `${(minute / 90) * 100}%`, transition: "width 1s ease" }} />
        </div>

        {/* xG */}
        <div className="mt-6">
          <div className="flex justify-between text-xs muted mb-2">
            <span>xG 1.84</span>
            <span>Ожидаемые голы</span>
            <span>0.97</span>
          </div>
          <div className="flex gap-1" style={{ height: 8 }}>
            <div className="grad-bg" style={{ width: "65%", borderRadius: 99 }} />
            <div style={{ width: "35%", borderRadius: 99, background: "#4de1ff" }} />
          </div>
        </div>

        {/* events */}
        <div className="mt-6 flex flex-col gap-3">
          {events.map((e) => (
            <div key={e.m} className="flex items-center gap-3 text-sm">
              <span className="muted tabular-nums" style={{ width: 34 }}>{e.m}</span>
              <span style={{ width: 8, height: 8, borderRadius: 99, background: e.c, flexShrink: 0 }} />
              <span className="font-semibold">{e.t}</span>
            </div>
          ))}
        </div>
      </div>
    </div>
  );
}

function Hero() {
  return (
    <header id="top" className="relative" style={{ minHeight: "100vh", overflow: "hidden" }}>
      {/* background */}
      <div className="hero-mask" style={{ position: "absolute", inset: 0 }}>
        <svg className="pitch-lines" viewBox="0 0 1200 800" preserveAspectRatio="xMidYMid slice" fill="none" stroke="#fff" strokeWidth="2">
          <rect x="60" y="60" width="1080" height="680" rx="6" />
          <line x1="600" y1="60" x2="600" y2="740" />
          <circle cx="600" cy="400" r="110" />
          <circle cx="600" cy="400" r="5" fill="#fff" />
          <rect x="60" y="230" width="190" height="340" />
          <rect x="950" y="230" width="190" height="340" />
          <rect x="60" y="320" width="70" height="160" />
          <rect x="1070" y="320" width="70" height="160" />
        </svg>
      </div>
      <div className="blob" style={{ width: 520, height: 520, top: -120, left: -140, background: "rgba(0,229,139,.35)" }} />
      <div className="blob" style={{ width: 460, height: 460, top: 160, right: -140, background: "rgba(216,245,59,.2)", animationDelay: "-5s" }} />
      <div className="blob" style={{ width: 380, height: 380, bottom: -100, left: "35%", background: "rgba(77,225,255,.15)", animationDelay: "-9s" }} />

      <div
        className="relative mx-auto px-6 grid lg:grid-cols-2 gap-16 items-center"
        style={{ maxWidth: 1200, minHeight: "100vh", paddingTop: 130, paddingBottom: 120, zIndex: 2 }}
      >
        <div>
          <div
            className="fiu d1 glass inline-flex items-center gap-3 px-4 mb-8 text-sm font-semibold"
            style={{ borderRadius: 999, minHeight: 40 }}
          >
            <span className="live-dot" />
            Сейчас в эфире: 14 матчей
          </div>

          <h1 className="h-mega">
            <span className="fiu d2" style={{ display: "block" }}>Гол забит.</span>
            <span className="fiu d3 grad-text" style={{ display: "block", paddingRight: ".1em" }}>Ты уже знаешь.</span>
          </h1>

          <p className="fiu d4 muted text-base md:text-xl mt-8" style={{ maxWidth: 520, lineHeight: 1.65 }}>
            Новости, live-счёт и честные инсайды по трансферам. Выберите свои клубы — и получайте только то, что действительно важно, на 15 секунд быстрее остальных.
          </p>

          <div className="fiu d5 flex flex-col sm:flex-row gap-4 mt-10">
            <a href="#cta" className="btn btn-grad">
              Читать бесплатно <ArrowRight size={18} />
            </a>
            <a href="#features" className="btn btn-ghost">
              <PlayCircle size={18} /> Смотреть возможности
            </a>
          </div>
        </div>

        <div className="fiu d4">
          <LiveCard />
        </div>
      </div>

      {/* ticker */}
      <div
        className="marquee-wrap"
        style={{
          position: "absolute", left: 0, right: 0, bottom: 0, zIndex: 3, overflow: "hidden",
          borderTop: "1px solid var(--border)", background: "rgba(6,9,10,.6)",
          backdropFilter: "blur(12px)", WebkitBackdropFilter: "blur(12px)",
        }}
      >
        <div className="marquee-track">
          {[...TICKER, ...TICKER].map((t, i) => (
            <span key={i} className="flex items-center gap-3 text-sm font-semibold whitespace-nowrap" style={{ padding: "1rem 2rem" }}>
              <span style={{ width: 6, height: 6, borderRadius: 99, background: "var(--accent-from)" }} />
              {t}
            </span>
          ))}
        </div>
      </div>
    </header>
  );
}

function Features() {
  return (
    <section id="features" className="relative px-6 py-24 md:py-32">
      <div className="mx-auto" style={{ maxWidth: 1200 }}>
        <SectionHead
          title="Всё о футболе — без шума"
          sub="Шесть инструментов, которые превращают поток новостей в понятную картину вашего клуба."
        />
        <div className="grid md:grid-cols-2 lg:grid-cols-3 gap-6">
          {FEATURES.map((f, i) => {
            const Icon = f.icon;
            return (
              <Reveal key={f.title} delay={i * 110}>
                <div className="glass card-hover p-8 h-full">
                  <div
                    className="flex items-center justify-center mb-6"
                    style={{
                      width: 56, height: 56, borderRadius: 18,
                      background: "linear-gradient(135deg, rgba(0,229,139,.2), rgba(216,245,59,.12))",
                      border: "1px solid rgba(0,229,139,.35)", color: "var(--accent-from)",
                    }}
                  >
                    <Icon size={26} />
                  </div>
                  <h3 className="text-xl font-bold mb-3">{f.title}</h3>
                  <p className="muted" style={{ lineHeight: 1.65 }}>{f.text}</p>
                </div>
              </Reveal>
            );
          })}
        </div>
      </div>
    </section>
  );
}

function Stats() {
  return (
    <section id="stats" className="relative">
      <div className="divider" />
      <div className="px-6 py-20 md:py-28" style={{ background: "linear-gradient(180deg, rgba(0,229,139,.05), transparent)" }}>
        <div className="mx-auto grid grid-cols-2 lg:grid-cols-4 gap-y-12 gap-x-6" style={{ maxWidth: 1200 }}>
          {STATS.map((s, i) => (
            <Reveal key={s.label} delay={i * 120} className="text-center">
              <div className="font-display grad-text" style={{ fontSize: "clamp(3.2rem, 8vw, 6rem)", fontWeight: 800, lineHeight: 1 }}>
                <Counter value={s.value} decimals={s.decimals} suffix={s.suffix} />
              </div>
              <div className="muted mt-3 text-sm md:text-base">{s.label}</div>
            </Reveal>
          ))}
        </div>
      </div>
      <div className="divider" />
    </section>
  );
}

function HowItWorks() {
  return (
    <section id="how" className="relative px-6 py-24 md:py-32">
      <div className="mx-auto" style={{ maxWidth: 1100 }}>
        <SectionHead title="Три шага до первого пуша" sub="Настройка занимает меньше минуты, регистрация не нужна." />
        <div className="relative">
          {/* desktop connector */}
          <div
            className="hidden md:block"
            style={{
              position: "absolute", top: 36, left: "16.6%", right: "16.6%", height: 2,
              background: "linear-gradient(90deg, var(--accent-from), var(--accent-to))", opacity: .5,
              animation: "glowPulse 3s ease-in-out infinite",
            }}
          />
          {/* mobile connector */}
          <div
            className="md:hidden"
            style={{
              position: "absolute", top: 36, bottom: 36, left: 35, width: 2,
              background: "linear-gradient(180deg, var(--accent-from), var(--accent-to))", opacity: .5,
            }}
          />
          <div className="grid md:grid-cols-3 gap-12 md:gap-8">
            {STEPS.map((s, i) => {
              const Icon = s.icon;
              return (
                <Reveal key={s.title} delay={i * 160}>
                  <div className="flex md:flex-col md:items-center md:text-center gap-6 md:gap-0">
                    <div className="relative" style={{ zIndex: 2, flexShrink: 0 }}>
                      <div
                        className="grad-bg flex items-center justify-center font-display"
                        style={{
                          width: 72, height: 72, borderRadius: 999, color: "#04140c",
                          fontSize: "2.2rem", fontWeight: 800,
                          boxShadow: "0 0 0 8px var(--bg-primary), 0 0 40px -4px rgba(0,229,139,.6)",
                        }}
                      >
                        {i + 1}
                      </div>
                    </div>
                    <div className="md:mt-8">
                      <div
                        className="hidden md:flex items-center justify-center mx-auto mb-4"
                        style={{ width: 44, height: 44, borderRadius: 14, color: "var(--accent-from)", background: "var(--card-bg)", border: "1px solid var(--border)" }}
                      >
                        <Icon size={22} />
                      </div>
                      <h3 className="text-xl font-bold mb-2">{s.title}</h3>
                      <p className="muted" style={{ lineHeight: 1.65, maxWidth: 320 }}>{s.text}</p>
                    </div>
                  </div>
                </Reveal>
              );
            })}
          </div>
        </div>
      </div>
    </section>
  );
}

function Testimonials() {
  return (
    <section id="reviews" className="relative px-6 py-24 md:py-32">
      <div className="blob" style={{ width: 420, height: 420, right: -160, top: "20%", background: "rgba(0,229,139,.12)" }} />
      <div className="mx-auto relative" style={{ maxWidth: 1200, zIndex: 1 }}>
        <SectionHead title="Те, кто читает нас первыми" sub="Редакторы, аналитики и обычные болельщики о том, как Свисток меняет их матчдей." />
        <div className="scroll-x flex md:grid md:grid-cols-3 gap-6 overflow-x-auto md:overflow-visible pb-4">
          {REVIEWS.map((r, i) => (
            <Reveal
              key={r.name}
              delay={i * 130}
              className="snap-center"
              style={{ minWidth: "85%", flexShrink: 0 }}
            >
              <div className="glass card-hover p-8 h-full flex flex-col">
                <div className="flex gap-1 mb-5">
                  {[0, 1, 2, 3, 4].map((s) => (
                    <Star key={s} size={18} fill="#d8f53b" color="#d8f53b" />
                  ))}
                </div>
                <p style={{ lineHeight: 1.7, fontSize: "1.02rem" }}>«{r.text}»</p>
                <div className="flex items-center gap-4 mt-auto pt-8">
                  <Avatar initials={r.initials} from={r.from} to={r.to} />
                  <div>
                    <div className="font-bold">{r.name}</div>
                    <div className="muted text-sm">{r.role}</div>
                  </div>
                </div>
              </div>
            </Reveal>
          ))}
        </div>
      </div>
      <style>{`@media (min-width: 768px){ #reviews .snap-center > div { } #reviews .snap-center { min-width: 0 !important; } }`}</style>
    </section>
  );
}

function Pricing() {
  const [yearly, setYearly] = useState(false);
  const fmt = (n) => n.toLocaleString("ru-RU");

  return (
    <section id="pricing" className="relative px-6 py-24 md:py-32">
      <div className="mx-auto" style={{ maxWidth: 1200 }}>
        <SectionHead title="Выберите свой тариф" sub="Начните бесплатно. Переходите на «Про», когда захотите видеть больше, чем остальные." />

        <Reveal className="flex justify-center" style={{ marginTop: "-1.5rem", marginBottom: "3.5rem" }}>
          <div className="toggle-wrap">
            <div
              className="toggle-pill grad-bg"
              style={{ left: yearly ? "calc(50% + 0px)" : 5, width: "calc(50% - 5px)" }}
            />
            <button className="toggle-btn" onClick={() => setYearly(false)} style={{ color: yearly ? "var(--muted)" : "#04140c" }}>
              Месяц
            </button>
            <button className="toggle-btn" onClick={() => setYearly(true)} style={{ color: yearly ? "#04140c" : "var(--muted)" }}>
              Год
              <span
                className="text-xs font-bold"
                style={{
                  padding: "2px 8px", borderRadius: 99,
                  background: yearly ? "rgba(4,20,12,.15)" : "rgba(0,229,139,.18)",
                  color: yearly ? "#04140c" : "var(--accent-from)", transition: "all .3s",
                }}
              >
                −20%
              </span>
            </button>
          </div>
        </Reveal>

        <div className="grid lg:grid-cols-3 gap-8 items-center">
          {PLANS.map((p, i) => {
            const price = yearly ? Math.round(p.monthly * 0.8) : p.monthly;
            return (
              <Reveal key={p.name} delay={i * 130}>
                <div className={`glass card-hover p-8 md:p-9 relative ${p.popular ? "price-pop" : ""}`}>
                  {p.popular && (
                    <div
                      className="grad-bg text-xs font-bold absolute"
                      style={{ top: -14, left: "50%", transform: "translateX(-50%)", padding: "6px 16px", borderRadius: 99, color: "#04140c", whiteSpace: "nowrap" }}
                    >
                      Популярный
                    </div>
                  )}
                  <h3 className="font-display" style={{ fontSize: "2rem", fontWeight: 800 }}>{p.name}</h3>
                  <p className="muted text-sm mt-1">{p.desc}</p>

                  <div className="flex items-end gap-2 mt-7" style={{ minHeight: 64 }}>
                    {p.monthly === 0 ? (
                      <span className="font-display" style={{ fontSize: "3.6rem", fontWeight: 800, lineHeight: 1 }}>0 ₽</span>
                    ) : (
                      <>
                        <span className="font-display tabular-nums" style={{ fontSize: "3.6rem", fontWeight: 800, lineHeight: 1 }}>
                          {fmt(price)} ₽
                        </span>
                        <span className="muted mb-2">/ мес</span>
                      </>
                    )}
                  </div>
                  <div className="muted text-sm mt-2" style={{ minHeight: 20 }}>
                    {p.monthly === 0
                      ? "Бесплатно навсегда"
                      : yearly
                      ? `${fmt(price * 12)} ₽ в год — выгода ${fmt(p.monthly * 12 - price * 12)} ₽`
                      : "Оплата помесячно"}
                  </div>

                  <div className="divider my-7" style={{ opacity: 0.35 }} />

                  <ul className="flex flex-col gap-4 mb-9">
                    {p.features.map((f) => (
                      <li key={f} className="flex items-start gap-3 text-sm">
                        <span
                          className="flex items-center justify-center"
                          style={{ width: 22, height: 22, borderRadius: 99, flexShrink: 0, background: "rgba(0,229,139,.16)", color: "var(--accent-from)" }}
                        >
                          <Check size={14} strokeWidth={3} />
                        </span>
                        <span style={{ lineHeight: 1.5 }}>{f}</span>
                      </li>
                    ))}
                  </ul>

                  <a href="#cta" className={`btn ${p.popular ? "btn-grad" : "btn-ghost"}`} style={{ width: "100%" }}>
                    {p.cta}
                  </a>
                </div>
              </Reveal>
            );
          })}
        </div>
      </div>
    </section>
  );
}

function FaqItem({ item, open, onToggle }) {
  return (
    <div className={`glass faq-item ${open ? "open" : ""}`}>
      <button
        onClick={onToggle}
        aria-expanded={open}
        className="w-full flex items-center justify-between gap-4 text-left px-6 py-5"
        style={{ background: "transparent", border: 0, color: "inherit", minHeight: 64 }}
      >
        <span className="font-bold" style={{ lineHeight: 1.4 }}>{item.q}</span>
        <ChevronDown
          size={22}
          style={{
            flexShrink: 0, color: "var(--accent-from)",
            transform: open ? "rotate(180deg)" : "rotate(0deg)",
            transition: "transform .45s cubic-bezier(.2,.8,.2,1)",
          }}
        />
      </button>
      <div className="faq-body" style={{ gridTemplateRows: open ? "1fr" : "0fr" }}>
        <div style={{ overflow: "hidden" }}>
          <p className="muted px-6 pb-6" style={{ lineHeight: 1.7 }}>{item.a}</p>
        </div>
      </div>
    </div>
  );
}

function Faq() {
  const [openIdx, setOpenIdx] = useState(0);
  const left = FAQ.filter((_, i) => i % 2 === 0);
  const right = FAQ.filter((_, i) => i % 2 === 1);
  const renderCol = (items, offset) =>
    items.map((item, k) => {
      const idx = k * 2 + offset;
      return (
        <Reveal key={item.q} delay={k * 100}>
          <FaqItem item={item} open={openIdx === idx} onToggle={() => setOpenIdx(openIdx === idx ? -1 : idx)} />
        </Reveal>
      );
    });

  return (
    <section id="faq" className="relative px-6 py-24 md:py-32">
      <div className="mx-auto" style={{ maxWidth: 1100 }}>
        <SectionHead title="Частые вопросы" sub="Не нашли ответ? Напишите нам — отвечаем в течение дня." />
        <div className="grid md:grid-cols-2 gap-5 items-start">
          <div className="flex flex-col gap-5">{renderCol(left, 0)}</div>
          <div className="flex flex-col gap-5">{renderCol(right, 1)}</div>
        </div>
      </div>
    </section>
  );
}

function CtaBanner() {
  const [email, setEmail] = useState("");
  const [done, setDone] = useState(false);
  const [error, setError] = useState(false);

  const submit = () => {
    if (/^\S+@\S+\.\S+$/.test(email)) {
      setDone(true);
      setError(false);
    } else {
      setError(true);
    }
  };

  const Rings = ({ side }) => (
    <svg
      viewBox="0 0 300 300"
      fill="none"
      stroke="#04140c"
      strokeWidth="2"
      style={{
        position: "absolute", top: "50%", [side]: -90, width: 340, height: 340,
        transform: "translateY(-50%)", opacity: 0.18, pointerEvents: "none",
        animation: "spinSlow 60s linear infinite",
      }}
    >
      <circle cx="150" cy="150" r="140" />
      <circle cx="150" cy="150" r="95" />
      <circle cx="150" cy="150" r="50" />
      <line x1="150" y1="10" x2="150" y2="290" />
    </svg>
  );

  return (
    <section id="cta" className="px-6 py-12 md:py-20">
      <Reveal>
        <div
          className="grad-bg relative mx-auto text-center px-6 py-16 md:py-24"
          style={{ maxWidth: 1200, borderRadius: 40, overflow: "hidden", color: "#04140c" }}
        >
          <Rings side="left" />
          <Rings side="right" />
          <div className="relative" style={{ zIndex: 2 }}>
            <h2 className="h-section" style={{ fontSize: "clamp(2.6rem, 7vw, 5.2rem)", maxWidth: 800, margin: "0 auto" }}>
              Следующий гол — в вашем почтовом ящике
            </h2>
            <p className="mt-6 text-base md:text-lg mx-auto" style={{ maxWidth: 520, opacity: 0.8, lineHeight: 1.6 }}>
              Каждое утро — главные новости, итоги тура и инсайды дня. Без спама, отписаться можно в один клик.
            </p>

            {done ? (
              <div className="mt-10 inline-flex items-center gap-3 font-bold" style={{ padding: "14px 24px", borderRadius: 99, background: "#04140c", color: "var(--accent-from)" }}>
                <Check size={20} /> Готово! Проверьте почту — мы отправили подтверждение.
              </div>
            ) : (
              <>
                <div className="flex flex-col sm:flex-row gap-3 mt-10 mx-auto" style={{ maxWidth: 520 }}>
                  <input
                    type="email"
                    className="email-input"
                    placeholder="Ваш e-mail"
                    value={email}
                    aria-label="Ваш e-mail"
                    onChange={(e) => { setEmail(e.target.value); setError(false); }}
                    onKeyDown={(e) => e.key === "Enter" && submit()}
                    style={error ? { borderColor: "#b3001b", borderWidth: 2 } : undefined}
                  />
                  <button className="btn btn-dark" onClick={submit}>
                    <Mail size={18} /> Подписаться
                  </button>
                </div>
                {error && (
                  <p className="mt-3 text-sm font-semibold" style={{ color: "#7a0012" }}>
                    Проверьте адрес: он должен выглядеть как name@mail.com
                  </p>
                )}
              </>
            )}
          </div>
        </div>
      </Reveal>
    </section>
  );
}

function Footer() {
  return (
    <footer className="px-6 pt-20 pb-10" style={{ borderTop: "1px solid var(--border)" }}>
      <div className="mx-auto" style={{ maxWidth: 1200 }}>
        <div className="grid lg:grid-cols-6 gap-12">
          <div className="lg:col-span-2">
            <a href="#top" className="flex items-center gap-3">
              <span className="grad-bg flex items-center justify-center" style={{ width: 40, height: 40, borderRadius: 14, color: "#04140c" }}>
                <Radio size={22} strokeWidth={2.4} />
              </span>
              <span className="font-display text-2xl font-bold" style={{ letterSpacing: ".02em" }}>СВИСТОК</span>
            </a>
            <p className="muted mt-5 text-sm" style={{ maxWidth: 320, lineHeight: 1.7 }}>
              Футбольные новости, live-счёт и аналитика. Только проверенные факты и только то, что вам интересно.
            </p>
            <div className="flex gap-3 mt-7">
              {[
                { Icon: Twitter, label: "Twitter" },
                { Icon: Instagram, label: "Instagram" },
                { Icon: Linkedin, label: "LinkedIn" },
                { Icon: Github, label: "GitHub" },
              ].map(({ Icon, label }) => (
                <a key={label} href="#top" className="social" aria-label={label}>
                  <Icon size={18} />
                </a>
              ))}
            </div>
          </div>

          <div className="lg:col-span-4 grid grid-cols-2 md:grid-cols-4 gap-10">
            {FOOTER_GROUPS.map((g) => (
              <div key={g.title}>
                <div className="font-bold mb-4">{g.title}</div>
                <ul className="flex flex-col">
                  {g.links.map((l) => (
                    <li key={l}>
                      <a href="#top" className="foot-link">{l}</a>
                    </li>
                  ))}
                </ul>
              </div>
            ))}
          </div>
        </div>

        <div className="divider mt-16 mb-8" style={{ opacity: 0.3 }} />
        <div className="flex flex-col md:flex-row justify-between gap-3 text-sm muted">
          <span>© 2026 Свисток. Все права защищены.</span>
          <span>Сделано для тех, кто не пропускает матчи.</span>
        </div>
      </div>
    </footer>
  );
}

/* ------------------------------------------------------------------ */
/*  ROOT                                                               */
/* ------------------------------------------------------------------ */
export default function FootballNewsLanding() {
  return (
    <div className="fb-root">
      <style>{css}</style>
      <Navbar />
      <Hero />
      <main>
        <Features />
        <Stats />
        <HowItWorks />
        <Testimonials />
        <Pricing />
        <Faq />
        <CtaBanner />
      </main>
      <Footer />
    </div>
  );
}
