# RecipeLab — Recipe Finder & Nutrition Info

**Autor:** Matei Iuliana — Grupa 1146

## Introducere
RecipeLab este o aplicație web care permite utilizatorilor să caute rețete după ingrediente și să vizualizeze informații nutriționale, accesând date din două servicii cloud prin API REST.

**Link aplicație:** https://recipe-app-mi.netlify.app

**Video demo:** https://youtube.com/LINKUL-TAU

---

## Descrierea problemei
Utilizatorii care doresc să gătească nu au o soluție unificată care să le ofere simultan idei de rețete și informații nutriționale pentru ingredientele doriete. RecipeLab rezolvă această problemă agregând date din două surse cloud independente într-o singură interfață simplă.

---

## Descrierea API-urilor

### API 1 — TheMealDB
- **URL:** `https://www.themealdb.com/api/json/v1/1/`
- **Scop:** furnizează rețete, ingrediente, instrucțiuni și imagini
- **Autentificare:** nu este necesară (API public gratuit)

### API 2 — Open Food Facts
- **URL:** `https://world.openfoodfacts.org/cgi/search.pl`
- **Scop:** furnizează informații nutriționale (calorii, proteine, grăsimi, carbohidrați)
- **Autentificare:** nu este necesară (API public gratuit)

---

## Flux de date

### Metode HTTP utilizate
Ambele API-uri folosesc metoda **GET**.

### Exemplu request / response — TheMealDB

**Request:**
GET https://www.themealdb.com/api/json/v1/1/filter.php?i=chicken

**Response:**
```json
{
  "meals": [
    {
      "strMeal": "Chicken Alfredo",
      "strMealThumb": "https://www.themealdb.com/images/media/meals/xxxx.jpg",
      "idMeal": "52772"
    }
  ]
}
```

### Exemplu request / response — Open Food Facts

**Request:**
GET https://world.openfoodfacts.org/cgi/search.pl?search_terms=chicken&search_simple=1&action=process&json=1&page_size=1

**Response:**
```json
{
  "products": [
    {
      "product_name": "Chicken breast",
      "nutriments": {
        "energy-kcal_100g": 165,
        "proteins_100g": 31.0,
        "fat_100g": 3.6,
        "carbohydrates_100g": 0.0
      }
    }
  ]
}
```

### Autentificare și autorizare
Niciun serviciu nu necesită autentificare. Ambele sunt API-uri publice, accesibile direct prin HTTP GET fără token sau cheie API.

---

## Referințe
- TheMealDB API: https://www.themealdb.com/api.php
- Open Food Facts API: https://world.openfoodfacts.org/data
- MDN Web Docs — Fetch API: https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API
