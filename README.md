# Wallet Dinetta - Dashboard di Monitoraggio

## 📋 Panoramica del Progetto

**Repository:** `wallet-dinetta`  
**Tipo:** Single-page application (SPA) statica  
**Deployment:** GitHub Pages  
**URL Live:** https://dalmamrk.github.io/wallet-dinetta/  
**Linguaggi:** HTML, CSS, JavaScript (vanilla)  

## 🎯 Scopo e Funzionalità

Questa repository contiene una dashboard web per monitorare in tempo reale un wallet di criptovalute su HyperLiquid. Il progetto è stato creato per un esperimento di trading familiare della durata di 30 giorni (dal 22 novembre al 27 dicembre 2025).

### Obiettivi Principali:
1. **Monitoraggio trasparente**: Permettere ai membri della famiglia di visualizzare in tempo reale lo stato del wallet condiviso
2. **Semplicità di accesso**: Dashboard accessibile via browser senza necessità di installazione o autenticazione
3. **Aggiornamento automatico**: Dati live aggiornati ogni 60 secondi tramite API HyperLiquid
4. **Design responsive**: Interfaccia ottimizzata per desktop e mobile

## 🏗️ Architettura

### Struttura del Progetto
```
wallet-dinetta/
├── index.html          # File principale (HTML + CSS + JavaScript)
└── README.md          # Questo documento
```

### Componenti Tecnici

#### 1. Frontend (index.html)
- **HTML5**: Struttura semantica della pagina
- **CSS3**: Styling con gradient background, responsive design, animazioni
- **JavaScript ES6+**: Logica di business e chiamate API

#### 2. API Integration
- **Endpoint**: `https://api.hyperliquid.xyz/info`
- **Metodo**: POST
- **Tipo di richiesta**: `clearinghouseState`
- **Frequenza aggiornamenti**: 60 secondi

#### 3. Deployment
- **GitHub Pages**: Hosting automatico su push al branch `main`
- **Workflow**: Ogni commit attiva un deploy automatico
- **Cache busting**: Parametro `?v=X` per forzare refresh

## 📊 Dati Visualizzati

### Metriche Principali:
1. **Inizio esperimento**: Data di inizio (22 novembre 2025)
2. **Totale iniziale**: $172.00 (150 Euro) - deposito iniziale
3. **Totale Wallet corrente**: Valore real-time del portafoglio
   - Calcolo: `totalNtlPos + withdrawable`
   - Formato: USD con 2 decimali
4. **Timestamp**: Data e ora ultimo aggiornamento

### Link Esterni:
- **HyperScan**: Link diretto all'explorer di HyperLiquid per visualizzare dettagli completi delle transazioni

## 🔧 Dettagli Implementativi

### Configurazione Wallet
```javascript
const WALLET_ADDRESS = '0x16ca778798c45c0fc0dffaa60190026075c6630c';
const START_DATE = '22 novembre 2025';
const INITIAL_DEPOSIT = 172; // $172 = 150 Euro
```

### Logica di Calcolo Wallet
```javascript
const totalValue = (
  parseFloat(data.marginSummary?.totalNtlPos || 0) + 
  parseFloat(data.marginSummary?.withdrawable || 0)
) || 0;
```

**Nota di sicurezza**: Utilizzo di optional chaining (`?.`) e fallback (`|| 0`) per gestire valori null/undefined e prevenire errori NaN.

### Gestione Date/Tempo
- Formato italiano: `DD/MM ore HH:MM`
- Timezone: CET (Central European Time)
- Aggiornamento automatico ogni minuto

## 🎨 Design System

### Palette Colori:
- **Background**: Gradient viola (`#667eea` → `#764ba2`)
- **Card**: Bianco con ombra (`rgba(0,0,0,0.2)`)
- **Testo primario**: `#333`
- **Testo secondario**: `#666`, `#999`
- **Highlight**: `#667eea` (viola principale)

### Typography:
- **Font**: `-apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto`
- **Dimensioni**: Responsive (0.75em - 1.8em)

### Responsive Design:
- Mobile-first approach
- Padding e margini adattivi
- Layout centrato con max-width 500px

## 🔄 Flusso di Aggiornamento Dati

1. **Caricamento pagina**: 
   - Chiamata immediata a `fetchWalletData()`
   - Display "Caricamento dati..."

2. **Fetch API**:
   - POST request a HyperLiquid
   - Parsing risposta JSON
   - Calcolo `totalValue`
   - Rendering HTML dinamico

3. **Aggiornamento ciclico**:
   - `setInterval(fetchWalletData, 60000)` (60 secondi)
   - Aggiornamento timestamp
   - Update valori visualizzati

4. **Gestione errori**:
   - Try-catch block
   - Display messaggio errore in caso di fallimento
   - Log console per debugging

## 📝 Cronologia Modifiche Principali

### Commit History:

1. **Initial commit** (22 Nov 2025)
   - Setup base dell'applicazione
   - Integrazione API HyperLiquid

2. **Remove footer note and enhance total value calculation** (23 Nov 2025)
   - Rimossa nota footer per semplificazione UI
   - Migliorato calcolo `totalValue` con gestione robusta di valori null/undefined
   - Fix bug NaN display

3. **Change INITIAL_DEPOSIT from 150 to 172** (23 Nov 2025)
   - Aggiornato valore iniziale da $150 a $172
   - Aggiunta nota "(150 Euro)" sotto il valore iniziale
   - Migliorata visualizzazione conversione valuta

## 🐛 Bug Fixes e Miglioramenti

### Problema 1: Display "$NaN"
**Causa**: Valori `undefined` da API in alcuni campi  
**Soluzione**: Implementato optional chaining e fallback values
```javascript
// Prima (bug)
const totalValue = parseFloat(data.marginSummary.totalNtlPos) + parseFloat(data.marginSummary.withdrawable);

// Dopo (fix)
const totalValue = (parseFloat(data.marginSummary?.totalNtlPos || 0) + parseFloat(data.marginSummary?.withdrawable || 0)) || 0;
```

### Problema 2: Footer note ridondante
**Causa**: Testo esplicativo eccessivo per utenti familiari  
**Soluzione**: Rimosso div `footer-note` per UI più pulita

## 🔐 Considerazioni di Sicurezza

### Punti di Attenzione:
1. **Wallet address pubblico**: L'indirizzo del wallet è visibile nel codice e nella URL HyperScan
2. **Nessuna autenticazione**: Dashboard accessibile a chiunque abbia il link
3. **Read-only**: Nessuna possibilità di eseguire transazioni tramite dashboard
4. **API pubblica**: Utilizzo di endpoint pubblico di HyperLiquid senza API key

### Best Practices Adottate:
- Nessuna chiave privata nel codice
- Solo operazioni di lettura
- Nessun dato sensibile oltre all'indirizzo pubblico
- CORS gestito da HyperLiquid API

## 🧪 Testing

### Test Manuali Effettuati:
1. ✅ Verifica caricamento dati da API
2. ✅ Test calcolo totalValue con vari scenari
3. ✅ Verifica responsive design (mobile/desktop)
4. ✅ Test aggiornamento automatico ogni 60s
5. ✅ Verifica link esterno HyperScan
6. ✅ Cache busting con parametro URL

### Browser Compatibility:
- ✅ Chrome/Edge (Chromium-based)
- ✅ Firefox
- ✅ Safari (iOS/macOS)
- ✅ Mobile browsers

## 🚀 Deployment Process

1. **Push to main branch**
2. **GitHub Actions** avvia workflow automatico
3. **Build** (non necessario, file statico)
4. **Deploy** su GitHub Pages
5. **Live** in ~1-2 minuti

### URL Pattern:
- Production: `https://dalmamrk.github.io/wallet-dinetta/`
- Cache busting: `https://dalmamrk.github.io/wallet-dinetta/?v=X`

## 📦 Dipendenze

**Nessuna dipendenza esterna!**  
Il progetto utilizza solo JavaScript vanilla, HTML5 e CSS3 nativi.

### Vantaggi:
- Zero build time
- Nessun package.json
- Nessun node_modules
- Deploy istantaneo
- Massima compatibilità

## 🔍 Aree di Miglioramento Futuro

### Possibili Enhancement:

1. **Grafici storici**:
   - Implementare chart.js per visualizzare trend
   - Storico performance giornaliera

2. **Notifiche**:
   - Alert browser per variazioni significative
   - Threshold personalizzabili

3. **Multiple wallets**:
   - Support per monitorare più wallet
   - Comparison view

4. **Dark mode**:
   - Toggle tema scuro/chiaro
   - Preferenza salvata in localStorage

5. **Export dati**:
   - Download CSV delle transazioni
   - Report PDF periodici

6. **Internazionalizzazione**:
   - Support multi-lingua (IT/EN)
   - Formattazione valuta locale

## 📞 Contatti e Supporto

**Repository Owner**: @dalmamrk  
**Issues**: https://github.com/dalmamrk/wallet-dinetta/issues  
**License**: Non specificata (progetto personale/familiare)

## 📚 Documentazione API

### HyperLiquid API Reference:
- **Base URL**: `https://api.hyperliquid.xyz`
- **Endpoint utilizzato**: `/info`
- **Docs**: https://hyperliquid.gitbook.io/hyperliquid-docs/

### Request Example:
```javascript
fetch('https://api.hyperliquid.xyz/info', {
  method: 'POST',
  headers: {'Content-Type': 'application/json'},
  body: JSON.stringify({
    type: 'clearinghouseState',
    user: WALLET_ADDRESS
  })
})
```

### Response Structure:
```json
{
  "marginSummary": {
    "accountValue": "149.99",
    "totalNtlPos": "0.00",
    "withdrawable": "149.99",
    ...
  },
  ...
}
```

## 🏁 Conclusioni

Questo progetto rappresenta una soluzione semplice ed efficace per il monitoraggio trasparente di un wallet crypto condiviso. La scelta di utilizzare tecnologie web native garantisce massima compatibilità, facilità di manutenzione e deployment rapido.

L'implementazione è stata ottimizzata per robustezza (gestione errori) e user experience (aggiornamenti automatici, design responsive, feedback visivo).

---

**Ultimo aggiornamento**: 23 novembre 2025  
**Versione**: 1.0.0  
**Status**: ✅ Produzione
