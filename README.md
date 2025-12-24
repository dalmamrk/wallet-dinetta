# Wallet Dinetta - Dashboard di Monitoraggio

## 📋 Panoramica del Progetto

**Repository:** `wallet-dinetta`  
**Tipo:** Single-page application (SPA) statica  
**Deployment:** GitHub Pages  
**URL Live:** https://dalmamrk.github.io/wallet-dinetta/  
**Linguaggi:** HTML, CSS, JavaScript (vanilla)

## 🎯 Scopo e Funzionalità

Questa repository contiene una dashboard web per monitorare in tempo reale un wallet di criptovalute su HyperLiquid. Il progetto è nato per un esperimento di trading familiare (Novembre-Dicembre 2025).

### Obiettivi Principali:
1. **Monitoraggio trasparente**: Visualizzazione real-time del wallet condiviso
2. **Semplicità di accesso**: Dashboard accessibile via browser
3. **Aggiornamento automatico**: Dati live aggiornati ogni 5 secondi tramite API HyperLiquid e RPC (Arbitrum + Mainnet)
4. **Design Mobile-First**: Interfaccia ottimizzata per l'uso su smartphone come native app

## 🏗️ Architettura

### Struttura del Progetto
```
wallet-dinetta/
├── index.html          # File principale (HTML + CSS + JavaScript)
├── elsa.webp          # Assets grafici
└── README.md          # Questo documento
```

### Componenti Tecnici

#### 1. Frontend (index.html)
- **HTML5**: Struttura semantica
- **CSS3**: Stile "App-like" su mobile (full width, bianco), Card view su desktop
- **JavaScript ES6+**: Logica asincrona e gestione stato

#### 2. API Integration
- **HyperLiquid**: `https://api.hyperliquid.xyz/info` - Clearinghouse, Spot State & Market Prices
- **Arbitrum RPC**: `https://arb1.arbitrum.io/rpc` - `eth_call` per balance USDC/ETH su L2
- **Ethereum Mainnet RPC**: Multipli endpoint (LlamaRPC, PublicNode, etc.) per balance ETH su L1
- **Frequenza aggiornamenti**: 5 secondi

#### 3. Deployment
- **GitHub Pages**: Hosting statico
- **Sicurezza**: Nessuna chiave privata esposta. Solo indirizzi pubblici.

## 📊 Dati Visualizzati

### Metriche:
1. **Totale Depositato**: Somma dei depositi storici
2. **Tokens nel Portafoglio**:
   - ETH (Somma di Spot HyperLiquid + Arbitrum One + Ethereum Mainnet)
   - USDC (Somma di Spot HyperLiquid + Arbitrum One)
3. **Tokens Investiti**:
   - Posizioni aperte (es. Bitcoin) con dati dinamici (Cost Basis fetchata da API)
4. **Totale Wallet**: Valore aggregato in USD

## 🔧 Dettagli Implementativi

### Calcolo Totale
```javascript
const spotETH = hyperLiquidSpotETH + arbitrumETH + mainnetETH;
const walletTotal = totalUSDC + perpInvestment + (spotETH * ethPrice);
```

### UX Mobile-First
- **Meta Theme Color**: Integrazione con la UI del browser (`#ffffff`)
- **Layout**: Full-screen su mobile (<480px) senza margini, Card centrata su desktop
- **Touch Targets**: Pulsanti estesi per facilitare il tocco

## 📝 Cronologia Modifiche

### 24 Dicembre 2025 (Major Update)
- **Integrazione Mainnet**: Aggiunta lettura saldo ETH su Ethereum Mainnet
- **Dati Dinamici BTC**: Sostituito testo statico con calcolo automatico del "Cost Basis" da HyperLiquid
- **Mobile Experience**: Ridisegnato CSS per un look "Native App" su mobile (sfondo bianco, no bordi)
- **Cleanup**: Rimozione testi obsoleti e token non più attivi

### Versioni Precedenti
- **23 Nov 2025**: Setup iniziale, integrazione HyperLiquid, calcolo totale robusto.

## 🔐 Sicurezza
- **Codice pubblico**: L'indirizzo del wallet è visibile ("public key") per permettere il fetch dei dati.
- **Fondi sicuri**: Nessuna chiave privata o seed phrase è presente nel codice. Impossibile effettuare transazioni dalla dashboard.

---

**Ultimo aggiornamento**: 24 dicembre 2025  
**Stato**: ✅ Attivo
