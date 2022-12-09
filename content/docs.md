---
title: 'Tatacoa API'
metaTitle: 'Documentation for Tatacoa API'
metaDescription: 'Documentation for Tatacoa API'
---

# Autenticación

---

Todas la peticiones al API de Tatacoa Bitcoin deben autenticarse. Para esto se deben enviar las credenciales recibidas usando los headers `apiid` y `apikey`.

# Invoices

---

## Creación

| Método | Ruta        |
| ------ | ----------- |
| `POST` | `/invoices` |

## Payload

```
  {
    "amount": 250,
    "currency": "COP",
    "description": "tatacoa bitcoin invoice",
    "webhook": "https://webhook.com"
  }
```

## Lista

| Método | Ruta        |
| ------ | ----------- |
| `GET`  | `/invoices` |

## Detalles

| Método | Ruta                   |
| ------ | ---------------------- |
| `GET`  | `/invoices/:invoiceId` |

# Payments

---

## Creación

| Método | Ruta        |
| ------ | ----------- |
| `POST` | `/payments` |

## Payload

```
  {
    "invoice": "lnbc1150n1p3qhh...3pf2gpe3g2usn0qcz49gq6nqwxw"
  }
```

## Lista

| Método | Ruta        |
| ------ | ----------- |
| `GET`  | `/payments` |

## Detalles

| Método | Ruta                   |
| ------ | ---------------------- |
| `GET`  | `/payments/:paymentId` |

# Gifts

---

## Creación

| Método | Ruta     |
| ------ | -------- |
| `POST` | `/gifts` |

## Payload

```
  {
    "amount": 100,
    "currency": "COP",
    "description": "tatacoa bitcoin gift"
  }
```

## Lista

| Método | Ruta     |
| ------ | -------- |
| `GET`  | `/gifts` |

## Detalles

| Método | Ruta            |
| ------ | --------------- |
| `GET`  | `/gits/:giftId` |
