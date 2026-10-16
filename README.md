# Practitioner MSAL

Cliente en Python (Jupyter Notebook) para consultar el servicio **Practitioner**
(HL7 FHIR) del **Bus de Interoperabilidad del Ministerio de Salud de la Nación
(Argentina)**. Busca profesionales de la salud registrados en REFEPS por DNI,
CUIL, código REFEPS o ID interno.

## Requisitos

- Python 3.10+
- Un dominio habilitado en el Bus de Interoperabilidad (`DOMAIN_URL` y
  `DOMAIN_TOKEN_SECRET`, otorgados por el MSAL)

## Uso

```bash
pip install -r requirements.txt
cp .env.example .env      # completar con las credenciales del dominio
jupyter notebook Practitioner_MSAL.ipynb
```

```python
client = PractitionerClient(DOMAIN_URL, DOMAIN_TOKEN_SECRET)

mostrar_respuesta(client.buscar_por_dni("12345678"))
mostrar_respuesta(client.buscar_por_cuil("20-12345678-9"))
mostrar_respuesta(client.buscar_por_refeps("541012345678"))
mostrar_respuesta(client.buscar_por_id("1234567"))
```

## Cómo funciona

1. Se arma un JWT firmado con HS256 usando el secreto del dominio (`clientAssertion`).
2. `POST /bus-auth/v2/auth` lo intercambia por un `accessToken`.
3. `GET /fhir/Practitioner?identifier=...` con `Authorization: Bearer <token>`.

## Seguridad

- `.env` está en `.gitignore`: las credenciales no se versionan.
- Las respuestas incluyen datos personales. Limpiá los outputs de la notebook
  antes de commitear.

## Referencias

- *Guía sobre servicio Practitioner v2.0*, Ministerio de Salud de la Nación, septiembre 2025.
