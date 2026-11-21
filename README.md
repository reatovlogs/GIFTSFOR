import { useMemo, useState, type ReactNode } from 'react';
import { ClerkProvider, SignIn, SignUp } from '@clerk/react';
import { publishableKeyFromHost } from '@clerk/react/internal';
import { ruRU } from '@clerk/localizations';
import { shadcn } from '@clerk/themes';
import {
Heart,
Search,
ArrowUpRight,
X,
ShieldAlert,
Plus,
UserRound,
Sparkles,
Menu,
ImagePlus,
Check,
} from 'lucide-react';
import {
Link,
Route,
Router as WouterRouter,
Switch,
useLocation,
} from 'wouter';

const basePath = import.meta.env.BASE_URL.replace(//$/, '');
const clerkProxyUrl = import.meta.env.VITE_CLERK_PROXY_URL;
const clerkEnvKey = import.meta.env.VITE_CLERK_PUBLISHABLE_KEY;
const clerkPubKey = clerkEnvKey
? publishableKeyFromHost(window.location.hostname, clerkEnvKey)
: '';

const cover = ${basePath}/images/gifts-for-cover.webp;

type Gift = {
id: number;
name: string;
collection: string;
category: string;
price: string;
serial: string;
style: string;
token: string;
description: string;
};

const gifts: Gift[] = [
{
id: 1,
name: 'Плюшевая нежность',
collection: 'Мягкие друзья',
category: 'Для настроения',
price: '2 450',
serial: '#0318',
style: 'art-bear',
token: 'ПН',
description:
'Тёплый маленький талисман для коллекции. Иллюстративный пример карточки — не реальное предложение.',
},
{
id: 2,
name: 'Алый бант',
collection: 'Праздничный набор',
category: 'Праздничные',
price: '1 890',
serial: '#0087',
style: 'art-ribbon',
token: 'Б',
description:
'Подарочная лента с характером — яркий акцент в цифровой витрине.',
},
{
id: 3,
name: 'Букет воспоминаний',
collection: 'Цветочный клуб',
category: 'Для настроения',
price: '3 200',
serial: '#0142',
style: 'art-bouquet',
token: 'Ц',
description:
'Собранный вручную букетный образ для примера интерфейса.',
},
{
id: 4,
name: 'Звезда вечеринки',
collection: 'Ночной город',
category: 'Редкие',
price: '5 750',
serial: '#0024',
style: 'art-star',
token: 'З',
description:
'Золотой символ большого вечера. Показанная цена — только демонстрационный макет.',
},
{
id: 5,
name: 'Сердце к сердцу',
collection: 'Тёплые слова',
category: 'Праздничные',
price: '2 100',
serial: '#0511',
style: 'art-heart',
token: 'С',
description:
'Маленькое признание в аккуратной цифровой упаковке.',
},
{
id: 6,
name: 'Тихий вечер',
collection: 'Домашний ритуал',
category: 'Для настроения',
price: '1 540',
serial: '#0196',
style: 'art-candle',
token: 'ТВ',
description:
'Свеча для спокойной коллекционной полки.',
},
{
id: 7,
name: 'Морская коробка',
collection: 'Сокровища бухты',
category: 'Редкие',
price: '4 680',
serial: '#0009',
style: 'art-box',
token: 'МК',
description:
'Небольшая находка с морским настроением.',
},
{
id: 8,
name: 'Жемчужный час',
collection: 'Светлые вещи',
category: 'Праздничные',
price: '2 760',
serial: '#0273',
style: 'art-pearl',
token: 'Ж',
description:
'Перламутровая деталь коллекции, созданная для демонстрации каталога.',
},
];

const categories = [
'Все подарки',
'Для настроения',
'Праздничные',
'Редкие',
];

const telegramBot = 'https://t.me/mechagramauth_bot';

function Header() {
const [location] = useLocation();
const [menuOpen, setMenuOpen] = useState(false);

return (
<header className="topbar">
<Link href="/" className="brand" data-testid="link-home">
<img src={cover} alt="GIFTS FOR" />
<span>GIFTS FOR</span>
</Link>

<nav className="topnav" aria-label="Основная навигация">  
    <Link href="/" className={location === '/' ? 'active' : ''}>  
      Купить  
    </Link>  
    <Link  
      href="/sell"  
      className={location === '/sell' ? 'active' : ''}  
    >  
      Продать  
    </Link>  
    <Link  
      href="/profile"  
      className={location === '/profile' ? 'active' : ''}  
    >  
      Профиль  
    </Link>  
  </nav>  

  <div className="top-actions">  
    <Link  
      href="/profile"  
      className="icon-btn"  
      aria-label="Открыть профиль"  
      data-testid="link-profile"  
    >  
      <UserRound size={17} />  
    </Link>  

    <Link href="/sell" className="outline-btn">  
      <Plus size={15} /> Добавить подарок  
    </Link>  

    <Link href="/sign-in" className="quiet-btn">  
      Войти  
    </Link>  

    <button  
      type="button"  
      className="icon-btn mobile-menu"  
      aria-label={menuOpen ? 'Закрыть меню' : 'Открыть меню'}  
      aria-expanded={menuOpen}  
      data-testid="button-mobile-menu"  
      onClick={() => setMenuOpen((open) => !open)}  
    >  
      <Menu size={17} />  
    </button>  
  </div>  

  {menuOpen && (  
    <nav className="mobile-nav" aria-label="Мобильная навигация">  
      <Link href="/" onClick={() => setMenuOpen(false)}>  
        Купить  
      </Link>  
      <Link href="/sell" onClick={() => setMenuOpen(false)}>  
        Продать подарок  
      </Link>  
      <Link href="/profile" onClick={() => setMenuOpen(false)}>  
        Профиль  
      </Link>  
    </nav>  
  )}  
</header>

);
}

function GiftArtwork({
gift,
detail = false,
}: {
gift: Gift;
detail?: boolean;
}) {
return (
<div
className={gift-art ${gift.style}${detail ? ' detail-art' : ''}}
style={
{
'--art-bg': gift.id % 2 ? '#21171a' : '#17191b',
} as React.CSSProperties
}
>
<div className="gift-object">{gift.token}</div>
</div>
);
}

function GiftCard({
gift,
favorite,
onFavorite,
onOpen,
}: {
gift: Gift;
favorite: boolean;
onFavorite: (id: number) => void;
onOpen: (gift: Gift) => void;
}) {
return (
<article
className="gift-card"
onClick={() => onOpen(gift)}
data-testid={card-gift-${gift.id}}
tabIndex={0}
onKeyDown={(event) => {
if (event.key === 'Enter') onOpen(gift);
}}
>
<GiftArtwork gift={gift} />

<div className="gift-meta">  
    <div className="gift-line">  
      <div className="gift-title">{gift.name}</div>  

      <button  
        className={`heart-button${favorite ? ' fav' : ''}`}  
        aria-label={  
          favorite ? 'Убрать из избранного' : 'В избранное'  
        }  
        data-testid={`button-favorite-${gift.id}`}  
        onClick={(event) => {  
          event.stopPropagation();  
          onFavorite(gift.id);  
        }}  
      >  
        <Heart  
          size={16}  
          fill={favorite ? 'currentColor' : 'none'}  
        />  
      </button>  
    </div>  

    <div className="gift-collection">{gift.collection}</div>  

    <div className="gift-price-row">  
      <div className="gift-price">  
        {gift.price} <small>₽*</small>  
      </div>  
      <span className="serial">{gift.serial}</span>  
    </div>  
  </div>  
</article>

);
}

function GiftDetails({
gift,
onClose,
}: {
gift: Gift;
onClose: () => void;
}) {
return (
<div  
className="modal-backdrop"  
role="presentation"  
onClick={onClose}  
>
<section
className="detail-modal"
role="dialog"
aria-modal="true"
aria-labelledby="detail-title"
onClick={(event) => event.stopPropagation()}
>
<GiftArtwork gift={gift} detail />

<div className="detail-copy">  
      <button  
        className="modal-close"  
        aria-label="Закрыть"  
        data-testid="button-close-details"  
        onClick={onClose}  
      >  
        <X size={19} />  
      </button>  

      <span className="eyebrow">  
        Пример коллекционной карточки  
      </span>  
      <h2 id="detail-title">{gift.name}</h2>  
      <div className="gift-collection">{gift.collection}</div>  
      <p>{gift.description}</p>  

      <div className="detail-stats">  
        <div className="detail-stat">  
          <span>Условная цена</span>  
          <b>{gift.price} ₽*</b>  
        </div>  
        <div className="detail-stat">  
          <span>Номер в примере</span>  
          <b>{gift.serial}</b>  
        </div>  
        <div className="detail-stat">  
          <span>Раздел витрины</span>  
          <b>{gift.category}</b>  
        </div>  
        <div className="detail-stat">  
          <span>Статус</span>  
          <b>Демо-данные</b>  
        </div>  
      </div>  

      <div className="detail-disclaimer">  
        Это макет карточки, а не активное объявление. Покупка,  
        оплата и передача подарков на сайте не выполняются.  
      </div>  
    </div>  
  </section>  
</div>

);
}

function Home() {
const [search, setSearch] = useState('');
const [category, setCategory] = useState('Все подарки');
const [sort, setSort] = useState('recommended');

const [favorites, setFavorites] = useState<number[]>(() => {
try {
const saved = JSON.parse(
localStorage.getItem('gf-favorites') || '[]',
);
return Array.isArray(saved)
? saved.filter((value): value is number => typeof value === 'number')
: [];
} catch {
return [];
}
});

const [selected, setSelected] = useState<Gift | null>(null);

const filtered = useMemo(() => {
const list = gifts.filter(
(gift) =>
(category === 'Все подарки' || gift.category === category) &&
${gift.name} ${gift.collection}
.toLowerCase()
.includes(search.toLowerCase()),
);

if (sort === 'price') {  
  list.sort(  
    (a, b) =>  
      Number(a.price.replace(/\s/g, '')) -  
      Number(b.price.replace(/\s/g, '')),  
  );  
}  

if (sort === 'price-desc') {  
  list.sort(  
    (a, b) =>  
      Number(b.price.replace(/\s/g, '')) -  
      Number(a.price.replace(/\s/g, '')),  
  );  
}  

return list;

}, [category, search, sort]);

const toggleFavorite = (id: number) => {
setFavorites((current) => {
const next = current.includes(id)
? current.filter((item) => item !== id)
: [...current, id];

localStorage.setItem('gf-favorites', JSON.stringify(next));  
  return next;  
});

};

return (
<div className="site-shell">
<Header />

<main className="page-wrap">  
    <section className="hero">  
      <div className="hero-copy">  
        <span className="eyebrow">  
          Твоя полка Telegram-подарков  
        </span>  

        <h1>  
          Вещи, которые  
          <br />  
          хочется <em>собирать.</em>  
        </h1>  

        <p>  
          GIFTS FOR — живая витрина цифровых подарков:  
          рассматривать, сохранять любимое и собирать свою  
          коллекционную историю.  
        </p>  

        <div  
          style={{  
            display: 'flex',  
            gap: 10,  
            flexWrap: 'wrap',  
            marginTop: 24,  
          }}  
        >  
          <a href="#catalog" className="primary-btn">  
            Смотреть витрину <ArrowUpRight size={15} />  
          </a>  
          <Link href="/sell" className="outline-btn">  
            Разместить подарок  
          </Link>  
        </div>  
      </div>  

      <div className="hero-visual">  
        <div className="hero-orbit" />  
        <div className="hero-orbit two" />  
        <img  
          className="hero-image"  
          src={cover}  
          alt="Иллюстрация GIFTS FOR"  
        />  
        <span className="hero-tag">КОЛЛЕКЦИОНИРУЙ СВОЁ</span>  
      </div>  
    </section>  

    <div  
      style={{  
        display: 'flex',  
        justifyContent: 'space-between',  
        alignItems: 'center',  
        flexWrap: 'wrap',  
        gap: 12,  
        marginTop: 21,  
      }}  
    >  
      <div className="notice">  
        <ShieldAlert size={15} /> Демо-витрина: все объявления  
        и цены — примеры  
      </div>  

      <a  
        className="quiet-btn"  
        href={telegramBot}  
        target="_blank"  
        rel="noopener noreferrer"  
        data-testid="link-telegram-bot"  
      >  
        Открыть Telegram-бота <ArrowUpRight size={13} />  
      </a>  
    </div>  

    <section id="catalog">  
      <div className="toolbar">  
        <label className="searchbox">  
          <Search size={16} />  
          <input  
            type="search"  
            value={search}  
            onChange={(event) => setSearch(event.target.value)}  
            placeholder="Найти подарок или коллекцию"  
            aria-label="Поиск по подаркам"  
            data-testid="input-search"  
          />  
        </label>  

        <div className="filter-row">  
          {categories.map((item) => (  
            <button  
              key={item}  
              className={`filter-pill${category === item ? ' active' : ''}`}  
              data-testid={`filter-${item}`}  
              onClick={() => setCategory(item)}  
            >  
              {item}  
            </button>  
          ))}  
        </div>  

        <select  
          className="sort-select"  
          value={sort}  
          aria-label="Сортировка"  
          onChange={(event) => setSort(event.target.value)}  
          data-testid="select-sort"  
        >  
          <option value="recommended">Сначала интересное</option>  
          <option value="price">Сначала дешевле</option>  
          <option value="price-desc">Сначала дороже</option>  
        </select>  
      </div>  

      <div className="section-heading">  
        <h2>Свежая подборка</h2>  
        <span>{filtered.length} ВИЗУАЛЬНЫХ ПРИМЕРОВ</span>  
      </div>  

      <div className="gift-grid">  
        {filtered.length ? (  
          filtered.map((gift) => (  
            <GiftCard  
              key={gift.id}  
              gift={gift}  
              favorite={favorites.includes(gift.id)}  
              onFavorite={toggleFavorite}  
              onOpen={setSelected}  
            />  
          ))  
        ) : (  
          <div className="empty-state">  
            <Sparkles size={24} />  
            <p>Такого подарка пока не нашлось.</p>  
            <button  
              className="quiet-btn"  
              onClick={() => {  
                setSearch('');  
                setCategory('Все подарки');  
              }}  
            >  
              Сбросить поиск  
            </button>  
          </div>  
        )}  
      </div>  

      <p className="demo-caption">  
        * Все цены, номера, коллекции и карточки на этой странице —  
        иллюстративные данные прототипа. Здесь нет реальных  
        предложений, оплат, сделок или передачи Telegram-подарков.  
      </p>  
    </section>  

    <footer className="bottom-note">  
      <span>  
        GIFTS FOR · кураторская витрина Telegram collectible gifts  
      </span>  
      <span>  
        Сайт не запрашивает коды входа или номер телефона Telegram.  
        Бот — отдельный внешний сервис, не способ авторизации на сайте.  
      </span>  
    </footer>  

    {selected && (  
      <GiftDetails  
        gift={selected}  
        onClose={() => setSelected(null)}  
      />  
    )}  
  </main>  
</div>

);
}

type Draft = {
name: string;
collection: string;
category: string;
price: string;
description: string;
};

const blankDraft: Draft = {
name: '',
collection: '',
category: 'Для настроения',
price: '',
description: '',
};

function Sell() {
const [draft, setDraft] = useState<Draft>(() => {
try {
return {
...blankDraft,
...(JSON.parse(
localStorage.getItem('gf-sell-draft') || '{}',
) as Partial<Draft>),
};
} catch {
return blankDraft;
}
});

const [saved, setSaved] = useState(false);

const change = (key: keyof Draft, value: string) => {
const next = { ...draft, [key]: value };
setDraft(next);
localStorage.setItem('gf-sell-draft', JSON.stringify(next));
setSaved(false);
};

return (
<div className="site-shell">
<Header />

<main className="page-wrap" style={{ paddingTop: 50 }}>  
    <span className="eyebrow">Черновик объявления</span>  
    <h1 className="page-title">Поделись находкой.</h1>  

    <p className="subcopy">  
      Заполни карточку для будущей витрины. Сейчас это только  
      локальный демо-черновик — он останется в этом браузере и  
      никуда не публикуется.  
    </p>  

    <div className="form-panel">  
      <div className="notice">  
        <ShieldAlert size={15} /> Форма не создаёт реальное  
        объявление, сделку или передачу подарка.  
      </div>  

      <form  
        onSubmit={(event) => {  
          event.preventDefault();  
          localStorage.setItem(  
            'gf-sell-draft',  
            JSON.stringify(draft),  
          );  
          setSaved(true);  
        }}  
      >  
        <div className="form-grid" style={{ marginTop: 25 }}>  
          <div className="field">  
            <label htmlFor="gift-name">Название подарка</label>  
            <input  
              id="gift-name"  
              required  
              maxLength={60}  
              value={draft.name}  
              onChange={(event) => change('name', event.target.value)}  
              placeholder="Например, Алый бант"  
              data-testid="input-gift-name"  
            />  
          </div>  

          <div className="field">  
            <label htmlFor="gift-collection">Коллекция</label>  
            <input  
              id="gift-collection"  
              value={draft.collection}  
              onChange={(event) =>  
                change('collection', event.target.value)  
              }  
              placeholder="Название коллекции"  
              data-testid="input-gift-collection"  
            />  
          </div>  

          <div className="field">  
            <label htmlFor="gift-category">Раздел</label>  
            <select  
              id="gift-category"  
              value={draft.category}  
              onChange={(event) =>  
                change('category', event.target.value)  
              }  
              data-testid="select-gift-category"  
            >  
              <option>Для настроения</option>  
              <option>Праздничные</option>  
              <option>Редкие</option>  
            </select>  
          </div>  

          <div className="field">  
            <label htmlFor="gift-price">Цена для макета, ₽</label>  
            <input  
              id="gift-price"  
              inputMode="decimal"  
              value={draft.price}  
              onChange={(event) => change('price', event.target.value)}  
              placeholder="Например, 2 450"  
              data-testid="input-gift-price"  
            />  
            <span className="field-hint">  
              Только для вашего черновика — сайт не принимает  
              оплату.  
            </span>  
          </div>  

          <div className="field full">  
            <label htmlFor="gift
