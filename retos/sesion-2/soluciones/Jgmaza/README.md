# Reto 3 — Consulta el estado del Pool

Script en TypeScript que lee el contrato del **Pool Comunitario** en testnet
y muestra en consola el estado de la campaña.

**Autor:** [Jgmaza](https://github.com/Jgmaza)  
**Contrato:** `CDF5IJWNCGNPBWTFCW3PWRXKPWZTPKQSHHRRYTSSCXKQ37U2CPDONSZ2`

## Qué hace

Llama (solo lectura, sin firmar) a:

- `name()` — nombre de la campaña
- `goal()` — meta en stroops
- `get_status()` — recaudado, meta, %, contribuyentes, deadline y reloj del ledger

Convierte stroops → XLM, basis points → porcentaje, y el deadline → días restantes.

## Cómo correrlo

Necesitas Node 18+.

```bash
npm install
npm run status
```

O directamente:

```bash
npx tsx pool-status.ts
```

No hace falta `.env` ni wallet: es solo simulación de lectura vía RPC.

## Ejemplo de salida

```
Campaña:          Stellar Campus CUC
Estado:           Retirada (admin ya sacó fondos)
Recaudado:        310 / 300 XLM
Meta (goal()):    300 XLM
Avance:           100.00%
Contribuyentes:   6
Días restantes:   1.7 días
```

(Los números cambian según el estado actual del contrato en testnet.)

## Cómo funciona (en corto)

1. Arma una transacción con `Contract.call("get_status")` (y lo mismo para `name` / `goal`).
2. La **simula** con el RPC de Soroban (`simulateTransaction`).
3. Decodifica el valor de retorno con `scValToNative`.
4. Formatea e imprime.

Por eso no pide clave secreta ni fondos: nunca se envía la tx a la red.
