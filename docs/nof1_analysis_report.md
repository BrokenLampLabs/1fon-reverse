# Reporte de Análisis de Seguridad: nof1.ai

**Target:** https://nof1.ai (PROYECTO AUTORIZADO)
**Fecha:** 2025-10-28
**Analista:** Claude Code - Reverse Engineer Agent
**Status:** Análisis Completo con Chrome DevTools
**ID de Scan:** nof1-ai-20251028-114405

---

## 1. Resumen Ejecutivo

**Alpha Arena** es una plataforma de benchmarking de IA que evalúa la capacidad de inversión de modelos de lenguaje (LLMs) usando **dinero real** en mercados reales de crypto perpetuals en Hyperliquid. Cada modelo recibe $10,000 USD de capital inicial y opera de forma autónoma.

### Stack Tecnológico Identificado
- **Framework:** Next.js (React)
- **Hosting:** Vercel
- **Fuentes:** Google Fonts (IBM Plex Mono)
- **Analíticas:** Vercel Analytics/Insights
- **API:** REST endpoints personalizados
- **Cache:** Vercel Edge Cache (HIT rate alto)

### Estado de Seguridad
- ✅ HTTPS habilitado con HSTS
- ✅ API endpoints protegidos con cache público
- ⚠️ Errores de React en consola (ver sección 6)
- ✅ Sin tokens de autenticación expuestos en esta vista pública

---

## 2. Endpoints de API Descubiertos

### 2.1 `/api/crypto-prices`
**Propósito:** Obtener precios en tiempo real de criptomonedas

**Request:**
```http
GET /api/crypto-prices HTTP/1.1
Host: nof1.ai
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7)
Referer: https://nof1.ai/
```

**Response:**
```http
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
Cache-Control: public, max-age=0, must-revalidate
X-Vercel-Cache: HIT
Server: Vercel
```

```json
{
  "prices": {
    "BTC": {"symbol": "BTC", "price": 114502.5, "timestamp": 1761651845673},
    "ETH": {"symbol": "ETH", "price": 4119.7, "timestamp": 1761651845673},
    "SOL": {"symbol": "SOL", "price": 199.885, "timestamp": 1761651845673},
    "BNB": {"symbol": "BNB", "price": 1135.65, "timestamp": 1761651845673},
    "DOGE": {"symbol": "DOGE", "price": 0.200035, "timestamp": 1761651845673},
    "XRP": {"symbol": "XRP", "price": 2.64375, "timestamp": 1761651845673}
  },
  "serverTime": 1761651845673
}
```

**Análisis:**
- ✅ Cache eficiente (age: 2s, X-Vercel-Cache: HIT)
- ✅ Timestamps Unix para sincronización
- 📊 Monedas soportadas: BTC, ETH, SOL, BNB, DOGE, XRP

---

### 2.2 `/api/trades`
**Propósito:** Historial completo de trades ejecutados por todos los modelos

**Request:**
```http
GET /api/trades HTTP/1.1
Host: nof1.ai
```

**Response:**
```http
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
Content-Encoding: br
Cache-Control: public, max-age=0, must-revalidate
X-Vercel-Cache: HIT
```

**Estructura de Trade:**
```json
{
  "id": "gpt-5_6a180304-15a6-4002-9032-7646c6abc4b6",
  "model_id": "gpt-5",
  "symbol": "SOL",
  "side": "long",
  "trade_type": "long",
  "quantity": 34.42,
  "entry_price": 203.56,
  "exit_price": 201.06,
  "entry_time": 1761641242.126,
  "exit_time": 1761646481.916,
  "entry_human_time": "2025-10-28 08:47:22.126000",
  "exit_human_time": "2025-10-28 10:14:41.916000",
  "leverage": 1,
  "realized_gross_pnl": -86.05,
  "realized_net_pnl": -89.683055,
  "total_commission_dollars": 3.633055,
  "entry_commission_dollars": 2.802613,
  "exit_commission_dollars": 0.830442,
  "confidence": 0,
  "entry_crossed": true,
  "exit_crossed": false,
  "entry_liquidation": null,
  "exit_liquidation": null
}
```

**Análisis:**
- 📊 Datos completos de entrada/salida
- 💰 Tracking de comisiones separadas (entry/exit)
- ⚡ Información de liquidación y cruce de spreads
- 🎯 Campo "confidence" (nivel de confianza del modelo)
- 🔢 Leverage tracking
- 📈 PnL bruto y neto calculado

---

### 2.3 `/api/account-totals`
**Propósito:** Estado actual de cuentas de todos los modelos con posiciones abiertas

**Request:**
```http
GET /api/account-totals HTTP/1.1
Host: nof1.ai
```

**Response:**
```http
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
Content-Encoding: br
X-Vercel-Cache: HIT
```

**Estructura de Cuenta (ejemplo GPT-5):**
```json
{
  "id": "gpt-5_0",
  "model_id": "gpt-5",
  "timestamp": 1760741958.847073,
  "realized_pnl": -32.909602,
  "total_unrealized_pnl": 337.88502,
  "dollar_equity": 10304.975418,
  "sharpe_ratio": 0.077,
  "cum_pnl_pct": 3.05,
  "since_inception_minute_marker": 59,
  "since_inception_hourly_marker": 0,
  "positions": {
    "XRP": {
      "symbol": "XRP",
      "entry_price": 2.339594,
      "current_price": 2.31705,
      "quantity": -7716,
      "leverage": 12,
      "margin": 1670.575901,
      "unrealized_pnl": 174.3401,
      "closed_pnl": -8.12,
      "commission": 16.243539,
      "liquidation_price": 2.4717151438,
      "risk_usd": 600,
      "confidence": 0.64,
      "exit_plan": {
        "profit_target": 2.19783,
        "stop_loss": 2.41611,
        "invalidation_condition": "Close if a 4h candle closes > 2.455 (20EMA + 1xATR14) AND the 4h MACD histogram turns >= 0."
      },
      "entry_time": 1760738675.497112,
      "entry_oid": 204600432746
    }
  }
}
```

**Análisis:**
- 🎯 Posiciones con leverage hasta 20x
- 📊 Sharpe ratio calculado en tiempo real
- 💰 Tracking de PnL realizado vs no realizado
- 🛡️ Liquidation prices calculados
- 📈 Exit plans con profit targets y stop losses
- 🤖 Niveles de confianza del modelo para cada trade
- ⚠️ **DATO CRÍTICO:** Posiciones short (quantity negativa) detectadas

---

### 2.4 `/api/since-inception-values`
**Propósito:** Valores iniciales y metadata de cada modelo desde inception

**Request:**
```http
GET /api/since-inception-values HTTP/1.1
Host: nof1.ai
```

**Response:**
```json
{
  "sinceInceptionValues": [
    {
      "id": "117506d4-d377-47b2-a90b-b86853f796d7",
      "model_id": "gpt-5",
      "nav_since_inception": 10000,
      "inception_date": 1760738409.834185,
      "num_invocations": 0
    },
    {
      "id": "3aa7df02-90f7-40a9-b996-7eafcfc0ac62",
      "model_id": "deepseek-chat-v3.1",
      "nav_since_inception": 10000,
      "inception_date": 1760738685.790017,
      "num_invocations": 0
    },
    {
      "id": "96cc37fa-da96-440f-91ad-5e3b6756baa9",
      "model_id": "claude-sonnet-4-5",
      "nav_since_inception": 10000,
      "inception_date": 1760739074.149193,
      "num_invocations": 0
    }
  ],
  "serverTime": 1761651549760
}
```

**Modelos en Competencia:**
- GPT-5
- DeepSeek Chat V3.1
- Grok 4
- Gemini 2.5 Pro
- Claude Sonnet 4.5
- Qwen3 Max
- BTC Buy&Hold (benchmark)

---

### 2.5 `/api/account-totals?lastHourlyMarker=253`
**Propósito:** Polling incremental para actualizaciones de cuentas

**Request:**
```http
GET /api/account-totals?lastHourlyMarker=253 HTTP/1.1
Host: nof1.ai
```

**Análisis:**
- 🔄 Sistema de polling con markers para datos incrementales
- ⚡ Optimización de bandwidth usando markers temporales
- 📡 Actualización en tiempo real del estado de cuentas

---

## 3. Recursos Estáticos y CDN

### 3.1 Assets de Next.js
```
/_next/static/css/*.css               - Estilos compilados
/_next/static/chunks/*.js             - JavaScript chunks
/_next/static/YEkf9XbCOC_cNu0U0LooY/ - Build ID específico
/_next/data/*.json                    - Data fetching de SSG
```

### 3.2 Imágenes y Logos
```
/logos/alpha%20logo.png              - Logo principal
/logos/favicon.png                   - Favicon
/logos/NOF1%20SQUARE%20BLACK.png     - Logo NOF1
/logos_white/*.png                   - Logos de modelos (GPT, Claude, etc.)
/coins/*.svg                         - Iconos de criptomonedas
```

### 3.3 Fuentes
```
Google Fonts: IBM Plex Mono (pesos: 100-700)
```

---

## 4. Vercel Analytics

### Endpoints de Telemetría
```http
POST /_vercel/insights/view
POST /_vercel/insights/event
GET  /_vercel/insights/script.js
```

**Análisis:**
- 📊 Telemetría de Vercel activada
- 🔍 Tracking de page views y eventos
- ⚠️ **PRIVACIDAD:** Datos de navegación compartidos con Vercel

---

## 5. Estructura de la Página

### Navegación Principal
```
- Alpha Arena (logo)
- LIVE
- LEADERBOARD
- BLOG
- MODELS (botón dropdown)
- JOIN THE PLATFORM WAITLIST
- ABOUT NOF1
```

### Secciones de Contenido
1. **Ticker de Precios:** BTC, ETH, SOL, BNB, DOGE, XRP
2. **Mejor/Peor Rendimiento:** DeepSeek Chat V3.1 (+116%) vs GPT 5 (-62.28%)
3. **Gráfico Principal:** Account value over time
4. **Controles:** ALL, 72H, $, %
5. **Cards de Modelos:** Ranking visual con valores de cuenta
6. **Tabs:** COMPLETED TRADES, MODELCHAT, POSITIONS, README.TXT

### Información del Proyecto
- Starting Capital: $10,000 por modelo
- Market: Crypto perpetuals en Hyperliquid
- Objective: Maximize risk-adjusted returns
- Transparency: Todos los outputs y trades son públicos
- Duration: Hasta November 3rd, 2025 a las 5 p.m. EST

---

## 6. Hallazgos de Seguridad

### 6.1 Errores de JavaScript (Consola)

**Errores Detectados:**
```
Minified React error #418 (14 ocurrencias)
Minified React error #423 (1 ocurrencia)
```

**Análisis:**
- ⚠️ React Error #418: "Hydration failed" - Mismatch entre SSR y cliente
- ⚠️ React Error #423: "Text content does not match"
- 🔍 **Impacto:** Problemas de renderizado, posibles inconsistencias en UI
- 📝 **Recomendación:** Revisar componentes con renderizado condicional

**Referencia:**
- https://reactjs.org/docs/error-decoder.html?invariant=418
- https://reactjs.org/docs/error-decoder.html?invariant=423

### 6.2 Seguridad de Headers

**Headers de Seguridad Presentes:**
```
✅ Strict-Transport-Security: max-age=63072000 (2 años)
✅ Content-Type con charset especificado
✅ HTTPS forzado
```

**Headers Ausentes (no críticos para API pública):**
```
⚠️ Content-Security-Policy
⚠️ X-Frame-Options
⚠️ X-Content-Type-Options
```

### 6.3 Autenticación y Autorización

**Observaciones:**
- ✅ No se detectaron tokens en endpoints públicos
- ✅ API de solo lectura expuesta públicamente (apropiado)
- 🔒 Endpoints de escritura probablemente protegidos (no descubiertos)
- ⚠️ **FALTA VERIFICAR:** Endpoints de login/signup, panel de administración

### 6.4 Rate Limiting

**Observaciones:**
- ✅ Cache agresivo en Vercel Edge (reduce carga)
- ⚠️ **NO DETECTADO:** Headers de rate limiting (X-RateLimit-*)
- 📝 **Recomendación:** Implementar rate limiting visible para transparencia

### 6.5 Exposición de Información

**Datos Sensibles Expuestos (por diseño):**
- ✅ Todas las posiciones de trading son públicas (transparencia)
- ✅ Todos los trades históricos son públicos
- ✅ Account values en tiempo real son públicos
- ✅ Exit plans con stop losses visibles

**Análisis:**
- ✅ **INTENCIONAL:** La transparencia es parte del proyecto
- ⚠️ Esto permite front-running si hay traders externos
- ⚠️ Stop losses públicos = vulnerabilidad potencial

---

## 7. Análisis de Performance

### Cache Performance
```
X-Vercel-Cache: HIT (mayoría de requests)
Age: 2-298 segundos
Cache-Control: public, max-age=0, must-revalidate
```

**Análisis:**
- ✅ Edge caching muy eficiente
- ✅ Revalidación constante para datos en tiempo real
- ✅ Estrategia stale-while-revalidate implícita

### Compresión
```
Content-Encoding: br (Brotli)
```

**Análisis:**
- ✅ Brotli usado para JSON responses grandes
- ✅ Reducción significativa de bandwidth

### ETag Usage
```
ETag: "69lj54s6w4c0" (crypto-prices)
ETag: W/"10mw37v3sdg4xf2" (trades - weak)
```

**Análisis:**
- ✅ ETags para validación de cache
- ✅ Weak ETags en datos dinámicos (apropiado)

---

## 8. Patrones de Arquitectura

### Frontend
- **Framework:** Next.js con SSR/SSG híbrido
- **Routing:** File-based routing
- **Data Fetching:** SWR o React Query probable (polling detectado)
- **State Management:** No observable desde exterior

### Backend
- **API:** REST endpoints en `/api/*`
- **Hosting:** Vercel Serverless Functions
- **Database:** No expuesto (probablemente PostgreSQL/MongoDB)
- **Cache:** Vercel Edge Network
- **Real-time:** Polling HTTP (no WebSockets detectado)

### Trading Infrastructure
- **Exchange:** Hyperliquid (crypto perpetuals)
- **Models:** 6 LLMs + 1 baseline (BTC Buy&Hold)
- **Execution:** Autónoma por cada modelo
- **Risk Management:** Leverage 1-20x, liquidation prices calculados

---

## 9. Vectores de Ataque Potenciales

### 9.1 Front-Running
**Severidad:** ALTA
**Descripción:** Exit plans públicos permiten que traders externos front-run las posiciones.

**Ejemplo:**
```json
"exit_plan": {
  "profit_target": 2.19783,
  "stop_loss": 2.41611,
  "invalidation_condition": "Close if 4h candle closes > 2.455..."
}
```

**Mitigación:**
- Ocultar exit plans hasta después de ejecución
- Usar órdenes ocultas en exchange
- Randomizar ligeramente los precios publicados

### 9.2 React Hydration Errors
**Severidad:** MEDIA
**Descripción:** Errores de hydration pueden causar inconsistencias en UI.

**Impacto:**
- Datos incorrectos mostrados temporalmente
- Posible XSS si hay sanitización inadecuada

**Mitigación:**
- Usar dev build para depurar
- Sincronizar timestamps entre servidor/cliente
- Validar formato de números en SSR

### 9.3 API Endpoint Discovery
**Severidad:** BAJA
**Descripción:** Endpoints adicionales pueden existir sin autenticación.

**Recomendación:**
- Fuzzing de `/api/*` endpoints
- Verificar `/api/admin`, `/api/internal`, etc.

### 9.4 Polling DoS
**Severidad:** BAJA
**Descripción:** Sin rate limiting visible, polling agresivo podría afectar performance.

**Mitigación:**
- Implementar rate limiting con headers visibles
- WebSockets para actualizaciones en tiempo real

---

## 10. Recomendaciones

### 10.1 Seguridad (Prioridad Alta)
1. ✅ **Mantener HSTS activo**
2. 🔧 **Agregar CSP headers** para prevenir XSS
3. 🔧 **Implementar rate limiting** con headers X-RateLimit-*
4. 🔍 **Auditar endpoints no públicos** (admin, internal)
5. 🛡️ **Considerar ocultar exit plans** hasta post-ejecución

### 10.2 Performance (Prioridad Media)
1. ✅ **Cache strategy es excelente** - mantener
2. 🔧 **Considerar WebSockets** para reducir polling overhead
3. 🔧 **Implementar delta updates** usando lastHourlyMarker más agresivamente

### 10.3 Desarrollo (Prioridad Media)
1. 🐛 **Fix React hydration errors** (#418, #423)
2. 📝 **Agregar source maps** solo en staging
3. 🧪 **Tests E2E** para validar consistency

### 10.4 Producto (Prioridad Baja)
1. 📊 **Métricas adicionales:** Win rate, max drawdown, Sortino ratio
2. 🔔 **Notificaciones:** Webhooks para trades completados
3. 📱 **Mobile optimization:** Responsive design review

---

## 11. Conclusiones

### Fortalezas
- ✅ **Transparencia total:** Todos los datos son públicos y verificables
- ✅ **Performance excelente:** Cache edge eficiente
- ✅ **Arquitectura moderna:** Next.js + Vercel stack
- ✅ **Seguridad básica:** HTTPS, HSTS correctamente configurado
- ✅ **API bien diseñada:** Endpoints lógicos y consistentes

### Debilidades
- ⚠️ **React errors en producción:** Afecta UX
- ⚠️ **Front-running risk:** Exit plans públicos
- ⚠️ **Sin rate limiting visible:** Potencial abuso
- ⚠️ **Falta CSP:** Protección adicional recomendada

### Impresión General
**nof1.ai/Alpha Arena** es una plataforma bien construida con un concepto innovador. La arquitectura es sólida y la transparencia es admirable. Las vulnerabilidades identificadas son mayormente de nivel bajo-medio y pueden ser mitigadas fácilmente.

**Calificación de Seguridad:** B+ (Notable)

---

## 12. Anexos

### A. Lista Completa de Network Requests
Ver archivo: `nof1_analysis_data.json` (68 requests capturados)

### B. Comandos para Replicar Análisis

```bash
# Usando Chrome DevTools Protocol
chrome --remote-debugging-port=9222
# Navegar a https://nof1.ai
# Exportar HAR file

# O usando curl
curl -H "User-Agent: Mozilla/5.0" https://nof1.ai/api/crypto-prices | jq
curl https://nof1.ai/api/trades | jq
curl https://nof1.ai/api/account-totals | jq
curl https://nof1.ai/api/since-inception-values | jq
```

### C. Python Script para Monitoreo Continuo

```python
import requests
import time
import json
from datetime import datetime

BASE_URL = "https://nof1.ai/api"

def fetch_prices():
    return requests.get(f"{BASE_URL}/crypto-prices").json()

def fetch_account_totals(marker=None):
    url = f"{BASE_URL}/account-totals"
    if marker:
        url += f"?lastHourlyMarker={marker}"
    return requests.get(url).json()

def monitor_loop(interval=10):
    """Monitor nof1.ai en tiempo real"""
    last_marker = None

    while True:
        try:
            prices = fetch_prices()
            accounts = fetch_account_totals(last_marker)

            print(f"\n[{datetime.now()}]")
            print(f"BTC: ${prices['prices']['BTC']['price']:,.2f}")

            for account in accounts.get('accountTotals', []):
                model = account['model_id']
                equity = account['dollar_equity']
                pnl = account['cum_pnl_pct']
                print(f"{model}: ${equity:,.2f} ({pnl:+.2f}%)")

            time.sleep(interval)

        except Exception as e:
            print(f"Error: {e}")
            time.sleep(interval)

if __name__ == "__main__":
    monitor_loop(interval=10)
```

---

**Fin del Reporte**

*Generado por: Claude Code - Reverse Engineer Agent*
*Fecha: 2025-10-28*
*Versión: 1.0*
