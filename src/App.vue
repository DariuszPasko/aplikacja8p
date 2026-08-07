<script setup>
import { computed, ref } from 'vue'

const activeView = ref('Pulpit')

const navigation = [
  { label: 'Pulpit', icon: '▦' },
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

const viewTitle = computed(() => activeView.value === 'Pulpit' ? 'Dzień dobry, Anna!' : activeView.value)
</script>

<template>
  <div class="app-shell">
    <aside class="sidebar">
      <a class="brand" href="#" @click.prevent="activeView = 'Pulpit'">
        <span class="brand-mark">8</span><span>pietro<span class="brand-dot">.</span></span>
      </a>
      <p class="sidebar-label">PANEL KLIENTA</p>
      <nav>
        <button v-for="item in navigation" :key="item.label" class="nav-item" :class="{ active: activeView === item.label }" @click="activeView = item.label">
          <span>{{ item.icon }}</span>{{ item.label }}
        </button>
      </nav>
      <div class="sidebar-bottom">
        <button class="nav-item"><span>?</span> Pomoc</button>
        <button class="profile"><span class="avatar">AK</span><span><strong>Anna Kowalska</strong><small>Klient</small></span><b>⌄</b></button>
      </div>
    </aside>

    <main>
      <header class="topbar">
        <button class="mobile-logo" @click="activeView = 'Pulpit'">8pietro.</button>
        <div class="breadcrumb">Panel klienta <span>/</span> {{ activeView }}</div>
        <div class="top-actions"><button class="bell" aria-label="Powiadomienia">♧<i></i></button><button class="avatar">AK</button></div>
      </header>

      <section class="content">
        <div class="heading-row">
          <div><h1>{{ viewTitle }}</h1><p v-if="activeView === 'Pulpit'">Oto najważniejsze informacje o Twoich usługach.</p><p v-else>Przeglądaj informacje dotyczące: {{ activeView.toLowerCase() }}.</p></div>
          <button class="outline-button">▢ Centrum pomocy</button>
        </div>

        <section v-if="activeView === 'Pulpit'" class="dashboard">
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
