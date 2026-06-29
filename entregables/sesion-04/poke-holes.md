# Historia 5.1 — Puntos más críticos

## Edge cases

- **Refresh de tokens:** la story guarda tokens pero no contempla qué pasa cuando el *access token* expira. ¿Hay refresh automático? ¿Se guarda el *refresh token*?
- **Revocación externa:** el usuario revoca el acceso desde su cuenta de Google (myaccount.google.com), no desde FlowSync. ¿Cómo detecta FlowSync que la conexión ya no es válida?

## Supuestos implícitos

- Que "**de forma segura**" significa algo concreto — pero no define dónde ni cómo (Keychain/Keystore en cliente vs. cifrado at-rest en backend).

## Escenarios faltantes

- **Flujo de refresh** y **re-autenticación** cuando el refresh token caduca o es revocado.

## Dependencias y riesgos no mencionados

- **Verificación de scopes sensibles de Google:** los scopes de Calendar son *sensitive/restricted*. Google exige verificación del OAuth consent screen y, para scopes restringidos, una evaluación de seguridad anual de un tercero. Esto es un bloqueante de tiempo/costo real, no un detalle técnico menor.
