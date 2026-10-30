<div align="center">

[<kbd> **🇬🇧 English** </kbd>](#english) <span>  </span> [<kbd> **🇷🇺 Русский** </kbd>](#russian)

</div>

---

<a name="english"></a>
# English

## SHIELD v4.99: Autonomous Derivative Risk-Management Framework

**Project Status:** Active R&D / Under Continuous Modernization. The project is open for collaboration and institutional infrastructure integration.



<p align="center">
  <img src="scheme.png" alt="SHIELD v4.99 Architecture Block Diagram" width="850">
</p>



### Executive Summary

SHIELD v4.99 is an institutional-grade, delta-neutral risk management architecture  [Bybit, 2026]. The system is engineered to automatically manage multi-asset structures on high-liquidity derivative desks (SOL and ETH networks) under the Bybit Unified Trading Account (UTA) operating with Portfolio Margin (TIMS stress-matrix netting) [Bybit, 2026].

The framework operates under a modular, highly decoupled architecture running inside a native Go compiled runtime [Bybit, 2026]. It prioritizes structural safety and capital preservation, designed to handle tail-risk dislocations with a massive margin of safety calibrated against historic market collapses like the FTX crisis [Bybit, 2026].

*Note: SHIELD operates as a completely autonomous, risk-insulated layer. It runs independently from our secondary aggressive engine, MONOLITH (adaptive premium harvesting), ensuring robust cross-strategic diversification [Bybit, 2026].*

### Capital Allocation & Dynamic Compounding

The runtime implements a dynamic, weekly (Monday-to-Monday) compounding mechanism that automatically scales trading parameters based on the live Account Ledger and Total Portfolio Equity (E) [Bybit, 2026]:

- **Options Collateral Segment:** Limited strictly to a maximum of 55% of total portfolio equity (`GOLimitRate = 0.55`) [Bybit, 2026].
- **Liquidity Reserve Segment:** Holds a mandatory 45% minimum buffer of total equity (`Cash_Buffer = 0.45`) [Bybit, 2026]. This capital is programmatically deployed into interest-bearing flexible vaults (Yield Netting) to offset execution drag while remaining fully credited as margin collateral in the broker's stress-testing matrix [Bybit, 2026].
- **Cross-Asset Margin Allocation:** Capital allocation limits between isolated subsystems of different assets are non-transferable to prevent cascading margin squeezes [Bybit, 2026].

### Pre-Trade Market Friction Filtration (MFG Engine)

Before structural positions are deployed or modified during the weekly initialization window (Mondays at 11:30 VLAT / 01:30 UTC), the state machine routes market parameters through a three-stage validation gateway called the **Market Friction Guard (MFG)** [Bybit, 2026]:

1. **Volatility Term Spread (`Ratio_TS`):** Evaluates the implied volatility ratio between short-dated front contracts and back-month tranches to prevent execution under abnormal inverted curve structures [Bybit, 2026].
2. **Market Maker Spread Velocity (`Vs`):** Tracks the derivative bid-ask spread expansion speed over a 10-minute trailing window [Bybit, 2026]. Entry is strictly blocked if `Vs > 0.35` [Bybit, 2026].
3. **Order Book Density-to-OI Slip (`OI_slip`):** Validates top-of-book market liquidity against the contract’s aggregate Open Interest to avoid transaction execution drag [Bybit, 2026].

Additionally, a pre-trade **TIMS Risk Parser** routine queries private REST ports to adjust entry parameters automatically if the clearinghouse tightens risk-matrix rules [Bybit, 2026].

### Intraday Delta-Netting & Tail-Risk Control

The active core aggregates the Greeks of all underlying option legs into a unified, thread-safe asynchronous matrix via the **State Core and Delta Netter**, computing total net portfolio delta (`D_Total`) in real time [Bybit, 2026]:

- **SOR Basis Router:** Monitors the annualized implied yield of linear instruments [Bybit, 2026]. Routes delta-hedging volume to quarterly delivery futures if basis divergence exceeds threshold limits to harvest convergence premiums; otherwise, liquidity clears via perpetual swaps [Bybit, 2026].
- **HFT Locked-Scalper:** A continuous, low-latency loop scalps micro-drifts in net delta using linear instruments, automatically tuning steps based on short-term gamma variance and VIP fee tiers [Bybit, 2026].
- **Delta-Flattening Override:** Violations of structural break-even boundaries suppress standard scalping and instantly trigger a hard-locking execution state, dropping active queues to prioritize cross-market delta immunization [Bybit, 2026].
- **Dynamic Margin Shield:** During backwardation or systemic risk shifts, the core activates a protective overlay designed to reduce TIMS maintenance margin requirements by **30-40%** at the exchange core layer, fully insulating the account during extreme spot crashes [Bybit, 2026].

### Preemptive Runtime Safety & Infrastructure

The infrastructure layer is developed on Golang to eliminate the four classical single points of failure inherent to crypto-derivative APIs [Bybit, 2026]:

- **Memory Protection (Deadlocks):** Global mutex usage is strictly forbidden [Bybit, 2026]. The state is protected via isolated synchronization primitives: `marginMu sync.RWMutex` (balances), `greeksMu sync.RWMutex` (net delta loops), and `orderMu sync.Mutex` (chaser transactions) with a 100ms WebSocket debounce filter [Bybit, 2026].
- **Rate-Limit Protocol Protection:** If order velocity surges toward exchange API limits (`f >= 45/sec`), the runtime switches to transactional bulk pipelines (`v5/order/create-batch`), preventing exchange disconnection [Bybit, 2026].
- **I/O Storage Buffer Overflow:** System state serialization (`state.json`) is managed through a non-blocking Go channel with an asynchronous `select` selector, preventing trading loop degradation [Bybit, 2026].
- **Order ID Sync:** The Mid-Price Chaser executes updates strictly via atomic `v5/order/amend` instructions directly inside the exchange matching engine's memory matrix, maintaining place-in-queue priorities [Bybit, 2026].
- **Dynamic Savings Liquidity Control:** If volatility indicators spike (`BBWP > 45`), the system automatically withdraws up to 70% of the flexible cash buffer back to the live trading account to backstop linear hedges [Bybit, 2026].

### Lifecycle Settlement & Emergency Breakers

- **Standard Settlement Route:** Front options expire automatically via the clearinghouse at Friday 18:00 VLAT / 5 DTE Expiry [Bybit, 2026]. At 18:00:05 VLAT, the runtime locks residual delta to absolute zero via perpetuals, later dissolving back-month long positions via progressive slices using the OTC Block Trading gateway based on top-of-book depth [Bybit, 2026].
- **Emergency Weekend OCO-Lock:** If Friday liquidity collapses (spreads > 35%) or API gateways fail, status `SLYSING_LIQUIDITY_CRISIS` engages [Bybit, 2026]. It freezes option queues, locks delta via a hard linear hedge, and deploys a sleeping OCO conditional pair directly to the exchange server node in Tokyo (0 ms network cross) [Bybit, 2026]. A scheduled **Liquidity-Driven Unwind Engine** executes a staggered liquidation loop on Monday morning at 11:00 VLAT to safely dismantle the lock by 11:20 VLAT [Bybit, 2026].

### Technical Stack

- **Language:** **Go (Golang)** — Goroutines, channels, sub-millisecond GC pauses.
- **API Protocol:** Protocol-buffers / JSON over WebSockets via **Bybit API v5** [Bybit, 2026].


---

<div align="center">

[<kbd> **🇬🇧 English** </kbd>](#english) <span>  </span> [<kbd> **🇷🇺 Русский** </kbd>](#russian)

</div>

<a name="russian"></a>
# Русский

## SHIELD v4.99: Автономный программный комплекс управления рисками деривативов

**Статус проекта:** Активный R&D / В процессе непрерывной модернизации. Проект открыт для совместной разработки и интеграции в институциональную инфраструктуру.


<p align="center">
  <img src="scheme.png" alt="Блок-схема архитектуры SHIELD v4.99" width="850">
</p>


### Краткое описание

SHIELD v4.99 — это дельта-нейтральная архитектура управления рисками  [Bybit, 2026]. Система спроектирована для автоматического управления мультиактивными структурами на высоколиквидных деривативных площадках (сетях SOL и ETH) внутри Единого торгового аккаунта Bybit в режиме Portfolio Margin (кросс-зачет маржи по стресс-матрицам TIMS) [Bybit, 2026].

Комплекс представляет собой модульную, слабосвязанную архитектуру, функционирующую внутри скомпилированной исполняемой среды на Go [Bybit, 2026]. Система ставит выживаемость капитала в абсолютный приоритет и спроектирована с жестким запасом прочности под сценарии рыночных коллапсов уровня краха биржи FTX [Bybit, 2026].

*Примечание: Система SHIELD полностью автономна и не связана с другими алгоритмами. Параллельно в квантовом стеке разработан второй независимый концепт — атакующий движок MONOLITH (адаптивный премиум-гарвестинг), обеспечивающий кросс-стратегическую диверсификацию [Bybit, 2026].*

### Динамический компаундинг и аллокация капитала

Рантайм реализует механизм еженедельного (Monday-to-Monday) пересчета торговых лимитов на основе фактического баланса аккаунта и общего значения Эквити портфеля (E) [Bybit, 2026]:

- **Сегмент обеспечения опционов:** Жестко ограничен максимальным значением в 55% от текущего общего Эквити аккаунта (`GOLimitRate = 0.55`) [Bybit, 2026].
- **Сегмент ликвидного резерва:** Удерживает обязательный маржинальный буфер в размере не менее 45% от общего Эквити (`Cash_Buffer = 0.45`) [Bybit, 2026]. Данный капитал программно размещается в гибких процентных смарт-пулах (Yield Netting) для снижения издержек на уплату биржевого трения, полностью сохраняя при этом статус зачетного маржинального обеспечения в стресс-матрице брокера [Bybit, 2026].
- **Кросс-активное распределение маржи:** Лимиты обеспечения между изолированными субсистемами разных активов жестко разграничены и не подлежат взаимному переливу во избежание каскадного маржинального сквиза [Bybit, 2026].

### Пре-трейд краш-фильтрация стакана (Механизм MFG)

Перед развертыванием или модификацией структурных позиций в понедельник в стартовое инициализационное окно (11:30 ВЛВ / 01:30 UTC), стейт-машина принудительно пропускает рыночные параметры через трехступенчатый шлюз валидации **Market Friction Guard (MFG)** [Bybit, 2026].

1. **Временной спред волатильности (`Ratio_TS`):** Оценивает соотношение подразумеваемой волатильности ближней и дальней серий контрактов для предотвращения входа в рынок в моменты аномальной инверсии кривой волатильности [Bybit, 2026].
2. **Скорость расширения спредов маркет-мейкеров (`Vs`):** Фиксирует динамику уширения спреда топ-брокеров за 10-минутный скользящий интервал [Bybit, 2026]. При пробое порога `Vs > 0.35` вход блокируется [Bybit, 2026].
3. **Плотность стакана к Открытому Интересу (`OI_slip`):** Проверяет объем ликвидности в Top-5 лимитных ордерах опционного деска относительно кумулятивного Open Interest выбранного страйка во избежание транзакционных потерь [Bybit, 2026].

Дополнительно пре-трейд горутина **TIMS Risk Parser** опрашивает приватные REST-порты спецификаций биржи, автоматически корректируя параметры входа, если клиринговый дом кулуарно ужесточает риск-параметры матриц [Bybit, 2026].

### Интрадей дельта-неттинг и хвостовой риск-контроль

Внутреннее ядро асинхронно агрегирует текущие греки всех опционных ног в единую потокобезопасную RAM-матрицу через модуль **State Core and Delta Netter**, вычисляя чистую дельту портфеля (`D_Total`) в режиме реального времени [Bybit, 2026]:

- **SOR Basis Router:** Смарт-роутер мониторит годовую процентную ставку контанго/бэквордации линейного рынка [Bybit, 2026]. При выходе базисного спреда за целевые лимиты дельта-хеджирование автоматически перенаправляется на Срочные Квартальные фьючерсы для фиксации премии схождения базиса; в остальных случаях объем очищается через бессрочные свопы (Perps) [Bybit, 2026].
- **HFT Locked-Scalper:** Низколатентный непрерывный поток скальпирует микро-дрейф нетто-дельты портфеля линейными фьючерсами, автоматически адаптируя шаг ордеров в зависимости от текущей интрадей-вариации Гаммы и VIP-тарифа аккаунта [Bybit, 2026].
- **Режим Delta-Flattening:** При форс-мажорном импульсном выходе спот-цены за расчетные границы безубыточности, логика скальпинга блокируется, а робот переходит в жесткий режим заморозки рисков, выравнивая кумулятивную дельту портфеля строго в ноль по рынку [Bybit, 2026].
- **Контур Dynamic Margin Shield:** В фазах бэквордации или системного изменения риск-векторов снижает поддерживающую маржу TIMS на 30-40% на уровне ядра биржи, полностью исключая маржин-колл аккаунта при экстремальных рыночных шоках спота на ±40% [Bybit, 2026].

### Превентивная защита рантайма и ликвидация точек отказа API

Инфраструктурный слой разработан на Golang с целью полной ликвидации четырех классических уязвимостей биржевых API-интерфейсов [Bybit, 2026]:

- **Защита памяти (Дедлоки):** Использование глобальных мьютексов категорически запрещено [Bybit, 2026]. Стейт разделен на изолированные примитивы синхронизации: `marginMu sync.RWMutex` (балансы), `greeksMu sync.RWMutex` (неттинг дельты), `orderMu sync.Mutex` (атомарность транзакций Chaser-а) с 100-миллисекундным дебаунс-фильтром на WebSocket [Bybit, 2026].
- **Протокольный WS Rate-Limiter:** При превышении частоты ордеров порога `f >= 45` запросов в секунду, рантайм склеивает одиночные лимитки в пакетные Bulk-транзакции (`v5/order/create-batch`), предотвращая сброс соединения биржей [Bybit, 2026].
- **Защита дискового буфера (I/O):** Сериализация состояния системы (`state.json`) вынесена в неблокирующий Go-канал со сквозным селектором `select`, исключая аппаратное зависание торговых горутин [Bybit, 2026].
- **Синхронизация Order ID:** Модуль **Mid-Price Chaser** осуществляет ведение ордера строго через метод `v5/order/amend` в мэтчинговом ядре биржи без снятия ордера из стакана, обнуляя риски потери места в очереди [Bybit, 2026].
- **Динамический пулл-контроль стейкинга:** При росте волатильности (`BBWP > 45`), система автоматически выводит до 70% свободного кэш-буфера из Bybit Savings обратно на баланс UTA для обеспечения встречных дельта-хеджей [Bybit, 2026].

### Клиринг сессий, экспирация и аварийные клапаны Токио

- **Штатный клиринговый маршрут:** Ближние ноги автоматически экспирируются биржевым движком в пятницу в 18:00 ВЛВ / 5 DTE Expiry [Bybit, 2026]. В 18:00:05 ВЛВ бот фиксирует чистую остаточную дельту дальних лонгов фьючерсным локом Bybit Perps до уровня строго 0.00, после чего они демонтируются слайсами через OTC-шлюз Block Trading v5 [Bybit, 2026].
- **Аварийный уик-энд ОСО-шлюз:** Если в пятницу к 19:58:50 ВЛВ стаканы пусты (маркет-спред > 35%) или API биржи лагает, активируется статус `SLYSING_LIQUIDITY_CRISIS` [Bybit, 2026]. Робот замораживает опционные очереди, фиксирует дельту лонгов фьючерсным локом и отправляет спящую пару OCO-условных ордеров напрямую на серверный узел Bybit в Токио (сетевой лаг 0 мс) [Bybit, 2026]. В понедельник в 11:00 ВЛВ активируется **Liquidity-Driven Unwind Engine**, пошагово и безопасно разбирающий замок к 11:20 ВЛВ [Bybit, 2026].

### Технологический Стек

- **Язык реализации:** **Go (Golang)** — горутины, каналы, суб-миллисекундные паузы GC.
- **Протокол интеграции:** Protocol-buffers / JSON через WebSockets с использованием **Bybit API v5** [Bybit, 2026].
