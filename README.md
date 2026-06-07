# Finální projekt č. 2 — Testování REST API

**Autor:** Kristína Belejčáková
**Datum:** 07.06.2026

---

## O projektu

Manuální testování REST API aplikace pro správu dat o studentech. Cílem bylo ověřit funkčnost všech čtyř HTTP metod (GET, POST, PUT, DELETE), validaci vstupů, konzistenci dat v databázi a správné chování při chybových stavech.

---

## Testované API

| Parametr | Hodnota |
|---|---|
| **Base URL** | `https://test-app.engeto.cz/students` |
| **Dokumentace** | `https://test-app.engeto.cz/docs` |
| **DB host** | `test-app.engeto.cz` |
| **DB port** | `5433` |
| **DB schema** | `students` |

### Endpointy

| Metoda | Endpoint | Popis |
|---|---|---|
| `GET` | `/students/{id}` | Získání studenta podle ID |
| `POST` | `/students` | Vytvoření nového studenta |
| `PUT` | `/students/{id}` | Částečná aktualizace studenta |
| `DELETE` | `/students/{id}` | Smazání studenta |

---

## Výsledky testování

| Metoda | Celkem TC | Passed | Failed |
|---|---|---|---|
| GET | 12 | 6 | 6 |
| DELETE | 12 | 5 | 7 |
| PUT | 26 | 19 | 7 |
| POST | 46 | 39 | 7 |
| **Celkem** | **96** | **69** | **27** |

**Nalezených bugů: 21**

---

## Struktura dokumentu

```
Testovani_REST_API_final_v3.docx
│
├── 1. Zadání
│   └── Popis projektu, přístupové údaje
│
├── 2. Testovací scénáře — GET (12 případů)
│   ├── 2.1 Pozitivní testy (TC_GET_01 – TC_GET_03)
│   └── 2.2 Negativní testy (TC_GET_04 – TC_GET_12)
│
├── 3. Testovací scénáře — DELETE (12 případů)
│   ├── 3.1 Pozitivní testy (TC_DEL_01 – TC_DEL_03)
│   └── 3.2 Negativní testy (TC_DEL_04 – TC_DEL_12)
│
├── 4. Testovací scénáře — PUT (26 případů)
│   ├── 4.1 Pozitivní testy (TC_PUT_01 – TC_PUT_12)
│   └── 4.2 Negativní testy (TC_PUT_13 – TC_PUT_26)
│
├── 5. Testovací scénáře — POST (46 případů)
│   ├── 5.1 Pozitivní testy (TC_POST_01 – TC_POST_23)
│   ├── 5.2 Negativní testy (TC_POST_24 – TC_POST_32)
│   └── 5.3 Hraniční testy (TC_POST_33 – TC_POST_46)
│
├── 6. Exekuce testů
│   └── Souhrnná tabulka všech 96 TC s výsledky (PASS/FAIL)
│
└── 7. Bug report
    └── 21 nalezených chyb s popisem, závažností a odkazem na TC
```

Každý testovací případ obsahuje:
- Kroky (číslované od 1 pro každý TC)
- URL a tělo requestu (Body)
- SQL příkaz pro DB verifikaci v DBeaver
- Očekávaný výsledek
- Skutečný výsledek
- Stav (Passed / Failed)

---

## Klíčové nálezy (výběr bugů)

| Bug ID | Závažnost | Popis |
|---|---|---|
| BUG_DEL_01 | 🔴 HIGH | DELETE vrací 200 OK, ale záznam v DB není smazán |
| BUG_DEL_05 | 🔴 HIGH | DELETE způsobuje poškození dat sousedního záznamu |
| BUG_GET_01 | 🔴 HIGH | GET neexistujícího ID vrací 500 místo 404 |
| BUG_POST_03 | 🔴 HIGH | isEuCitizen nevaliduje typ — přijímá string i integer |
| BUG_POST_07 | 🔴 HIGH | Extrémně vysoké numberOfFailedStudies způsobuje 500 |
| BUG_PUT_03 | 🔴 HIGH | PUT povoluje duplicitní email pro různé studenty |
| BUG_POST_01 | 🔴 HIGH | POST ignoruje pole zipCode |

Úplný seznam všech 21 bugů je v sekci **7. Bug report** dokumentu.

---

## Použité nástroje

| Nástroj | Verze / Poznámka |
|---|---|
| **Postman** | Odesílání HTTP requestů |
| **DBeaver** | Verifikace dat v PostgreSQL databázi |
| **API dokumentace** | Swagger UI na `/docs` |

---

## Autorizace

API využívá Bearer Token autorizaci.

```
POST https://test-app.engeto.cz/login
```

Token se přikládá ke každému requestu jako hlavička:

```
Authorization: Bearer <token>
```

Token nemá časové omezení platnosti.
