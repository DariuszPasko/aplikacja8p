<script setup>
import { computed, ref } from 'vue'

const activeView = ref('Pulpit')
const isAuthenticated = ref(false)
const email = ref('anna@twojafirma.pl')
const password = ref('demo1234')
const loginError = ref('')
const activeUser = ref(null)

const clientAccount = {
  name: 'Anna Kowalska',
  initials: 'AK',
  company: 'Twoja Firma Sp. z o.o.',
  email: 'anna@twojafirma.pl',
  role: 'Klient',
}

const adminAccount = {
  name: 'Dariusz Paśko',
  initials: 'DP',
  company: '8piętro',
  email: 'dariuszpasko@icloud.com',
  role: 'Administrator',
}

const clientNavigation = [
  { label: 'Pulpit', icon: '▦' },
  { label: 'Usługi', icon: '◫' },
  { label: 'Płatności', icon: '◌' },
  { label: 'SEO', icon: '↗' },
]

const adminNavigation = [
  { label: 'Pulpit', icon: '▦' },
  { label: 'Klienci', icon: '♙' },
  { label: 'Usługi', icon: '◫' },
  { label: 'Płatności', icon: '◌' },
  { label: 'SEO', icon: '↗' },
]

const services = [
  { type: 'Hosting', name: 'Hosting Business', domain: 'twojafirma.pl', renewal: '18 wrz 2026', price: '369 zł / rok', status: 'Aktywny' },
  { type: 'Domena', name: 'twojafirma.pl', domain: 'Domena .pl', renewal: '18 wrz 2026', price: '79 zł / rok', status: 'Aktywna' },
  { type: 'Domena', name: 'twojafirma.com', domain: 'Domena .com', renewal: '3 gru 2026', price: '89 zł / rok', status: 'Aktywna' },
]

const invoices = [
  { number: 'FV/08/2026/018', name: 'Odnowienie hostingu Business', date: '18.09.2026', amount: '369,00 zł', status: 'Do opłacenia' },
  { number: 'FV/07/2026/005', name: 'Odnowienie domeny twojafirma.pl', date: '18.09.2026', amount: '79,00 zł', status: 'Zaplanowana' },
]

const isAdmin = computed(() => activeUser.value?.role === 'Administrator')
const navigation = computed(() => isAdmin.value ? adminNavigation : clientNavigation)
const panelName = computed(() => isAdmin.value ? 'Panel administratora' : 'Panel klienta')
const viewTitle = computed(() => activeView.value === 'Pulpit' ? `Dzień dobry, ${activeUser.value?.name.split(' ')[0] || ''}!` : activeView.value)

function login() {
  const credentials = { email: email.value.trim().toLowerCase(), password: password.value }
  if (credentials.email === adminAccount.email && credentials.password === 'admin1234') {
    activeUser.value = adminAccount
  } else if (credentials.email === clientAccount.email && credentials.password === 'demo1234') {
    activeUser.value = clientAccount
  } else {
    loginError.value = 'Nieprawidłowe dane. Użyj jednego z kont demonstracyjnych.'
    return
  }

  loginError.value = ''
  isAuthenticated.value = true
}

function logout() {
  isAuthenticated.value = false
  activeUser.value = null
  activeView.value = 'Pulpit'
}
</script>

<template>
  <main v-if="!isAuthenticated" class="login-page">
    <section class="login-panel">
      <a class="login-brand" href="#" @click.prevent><span>8</span>pietro<span class="brand-dot">.</span></a>
      <div class="login-copy">
        <p class="eyebrow">PANEL KLIENTA</p>
        <h1>Witaj ponownie</h1>
        <p>Zaloguj się, aby sprawdzić usługi, płatności i raporty dla swojej firmy.</p>
      </div>
      <form class="login-form" @submit.prevent="login">
        <label for="email">Adres e-mail</label>
        <input id="email" v-model="email" type="email" autocomplete="email" placeholder="nazwa@firma.pl" />
        <div class="password-row"><label for="password">Hasło</label><button type="button">Nie pamiętam hasła</button></div>
        <input id="password" v-model="password" type="password" autocomplete="current-password" placeholder="••••••••" />
        <p v-if="loginError" class="login-error">{{ loginError }}</p>
        <button class="login-button" type="submit">Zaloguj się <span>→</span></button>
      </form>
      <p class="demo-note">Demo klienta: anna@twojafirma.pl / demo1234<br>Demo administratora: dariuszpasko@icloud.com / admin1234</p>
    </section>
    <aside class="login-aside"><div class="login-orb orb-one"></div><div class="login-orb orb-two"></div><div class="login-aside-content"><span class="aside-icon">◫</span><blockquote>„Wszystkie ważne informacje o Twojej obecności w sieci, w jednym miejscu.”</blockquote><p>8piętro · partner cyfrowy Twojej firmy</p></div></aside>
  </main>

  <div v-else class="app-shell">
    <aside class="sidebar">
      <a class="brand" href="#" @click.prevent="activeView = 'Pulpit'">
        <span class="brand-mark">8</span><span>pietro<span class="brand-dot">.</span></span>
      </a>
      <p class="sidebar-label">{{ panelName.toUpperCase() }}</p>
      <nav>
        <button v-for="item in navigation" :key="item.label" class="nav-item" :class="{ active: activeView === item.label }" @click="activeView = item.label">
          <span>{{ item.icon }}</span>{{ item.label }}
        </button>
      </nav>
      <div class="sidebar-bottom">
        <button class="nav-item"><span>?</span> Pomoc</button>
        <button class="profile" @click="logout" title="Wyloguj"><span class="avatar">{{ activeUser.initials }}</span><span><strong>{{ activeUser.name }}</strong><small>{{ activeUser.role }} · Wyloguj</small></span><b>⌄</b></button>
      </div>
    </aside>

    <main>
      <header class="topbar">
        <button class="mobile-logo" @click="activeView = 'Pulpit'">8pietro.</button>
        <div class="breadcrumb">{{ panelName }} <span>/</span> {{ activeView }}</div>
        <div class="top-actions"><button class="bell" aria-label="Powiadomienia">♧<i></i></button><button class="avatar" :title="activeUser.name">{{ activeUser.initials }}</button></div>
      </header>

      <section class="content">
        <div class="heading-row">
          <div><h1>{{ viewTitle }}</h1><p v-if="activeView === 'Pulpit'">{{ isAdmin ? 'Przegląd najważniejszych informacji o klientach i usługach.' : 'Oto najważniejsze informacje o Twoich usługach.' }}</p><p v-else>Przeglądaj informacje dotyczące: {{ activeView.toLowerCase() }}.</p></div>
          <button class="outline-button">▢ Centrum pomocy</button>
        </div>

        <section v-if="activeView === 'Pulpit' && isAdmin" class="dashboard admin-dashboard">
          <div class="admin-banner"><div><span class="eyebrow">KONTO GŁÓWNE</span><h2>Zarządzaj klientami z jednego miejsca</h2><p>Dodawaj usługi, kontroluj terminy odnowień i reaguj na płatności wymagające uwagi.</p></div><span class="admin-banner-icon">♙</span></div>
          <div class="stat-grid admin-stats">
            <article class="stat-card"><div class="stat-head"><span>Aktywni klienci</span><b class="blue-icon">♙</b></div><strong>12</strong><p>+2 w tym miesiącu</p></article>
            <article class="stat-card"><div class="stat-head"><span>Aktywne usługi</span><b class="violet-icon">◫</b></div><strong>34</strong><p>Hosting, domeny i SEO</p></article>
            <article class="stat-card"><div class="stat-head"><span>Płatności do uwagi</span><b class="orange-icon">!</b></div><strong>3</strong><p>Wymagają kontaktu</p></article>
          </div>
          <div class="section-title"><div><h2>Ostatnio aktywni klienci</h2><p>Szybki podgląd spraw wymagających działania.</p></div><button class="text-button" @click="activeView = 'Klienci'">Wszyscy klienci <span>→</span></button></div>
          <div class="table-wrap"><table><thead><tr><th>KLIENT</th><th>USŁUGI</th><th>NAJBLIŻSZE ODNOWIENIE</th><th>PŁATNOŚCI</th><th>STATUS</th></tr></thead><tbody><tr><td><strong>Anna Kowalska</strong><small class="table-subtitle">Twoja Firma Sp. z o.o.</small></td><td>3</td><td>18.09.2026</td><td>369,00 zł</td><td><span class="badge warning">Do kontaktu</span></td></tr><tr><td><strong>Michał Nowak</strong><small class="table-subtitle">Studio N</small></td><td>2</td><td>03.12.2026</td><td>—</td><td><span class="badge success">W porządku</span></td></tr><tr><td><strong>Julia Wiśniewska</strong><small class="table-subtitle">W Design</small></td><td>4</td><td>12.08.2026</td><td>149,00 zł</td><td><span class="badge warning">Termin dziś</span></td></tr></tbody></table></div>
        </section>

        <section v-else-if="activeView === 'Pulpit'" class="dashboard">
          <div class="alert"><span class="alert-icon">!</span><div><strong>Masz płatność do uregulowania</strong><p>Termin płatności za odnowienie hostingu mija za 41 dni.</p></div><button>Zobacz płatność <span>→</span></button></div>

          <div class="stat-grid">
            <article class="stat-card"><div class="stat-head"><span>Aktywne usługi</span><b class="blue-icon">◫</b></div><strong>3</strong><p>Hosting i domeny</p></article>
            <article class="stat-card"><div class="stat-head"><span>Najbliższe odnowienie</span><b class="orange-icon">◷</b></div><strong>18 wrz</strong><p>za 41 dni</p></article>
            <article class="stat-card"><div class="stat-head"><span>Wyniki SEO</span><b class="violet-icon">↗</b></div><strong>Wkrótce</strong><p>Moduł w przygotowaniu</p></article>
          </div>

          <div class="section-title"><div><h2>Twoje usługi</h2><p>Usługi przypisane do Twojego konta.</p></div><button class="text-button" @click="activeView = 'Usługi'">Zobacz wszystkie <span>→</span></button></div>
          <div class="service-grid">
            <article v-for="service in services" :key="service.name" class="service-card"><div class="service-icon" :class="service.type.toLowerCase()">{{ service.type === 'Hosting' ? '◫' : '◎' }}</div><div><span class="service-type">{{ service.type }}</span><h3>{{ service.name }}</h3><p>{{ service.domain }}</p></div><div class="service-meta"><span class="badge success">{{ service.status }}</span><p>Odnowienie: <strong>{{ service.renewal }}</strong></p></div></article>
          </div>

          <div class="section-title invoices-title"><div><h2>Nadchodzące płatności</h2><p>Opłać faktury na czas, aby zachować ciągłość usług.</p></div><button class="text-button" @click="activeView = 'Płatności'">Historia płatności <span>→</span></button></div>
          <div class="table-wrap"><table><thead><tr><th>NUMER FAKTURY</th><th>USŁUGA</th><th>TERMIN PŁATNOŚCI</th><th>KWOTA</th><th>STATUS</th><th></th></tr></thead><tbody><tr v-for="invoice in invoices" :key="invoice.number"><td><strong>{{ invoice.number }}</strong></td><td>{{ invoice.name }}</td><td>{{ invoice.date }}</td><td><strong>{{ invoice.amount }}</strong></td><td><span class="badge" :class="invoice.status === 'Do opłacenia' ? 'warning' : 'neutral'">{{ invoice.status }}</span></td><td><button class="pay-button">Opłać</button></td></tr></tbody></table></div>
        </section>

        <section v-else class="empty-state"><div>{{ activeView === 'SEO' ? '↗' : activeView === 'Płatności' ? '◌' : '◫' }}</div><h2>Widok „{{ activeView }}”</h2><p>Ten moduł jest gotowy do rozwinięcia o dane z Twojego systemu.</p></section>
      </section>
    </main>
  </div>
</template>
