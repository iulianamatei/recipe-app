# RecipeLab — Recipe Finder & Nutrition Info

**Autor:** Matei Iuliana — Grupa 1146

**Link aplicație:** https://recipe-app-mi.netlify.app
**Video demo:** https://youtu.be/3RAvpN1ywz8

---

## 1. Introducere

RecipeLab este o aplicație web care permite utilizatorilor să caute rețete după ingrediente și să vizualizeze informații nutriționale, accesând date din două servicii cloud prin API REST.

Aplicația integrează:
- **TheMealDB API** — pentru rețete, ingrediente și instrucțiuni de gătit
- **Open Food Facts API** — pentru informații nutriționale (calorii, proteine, grăsimi, carbohidrați, fibre)

Tehnologii folosite: HTML, CSS, JavaScript (vanilla), Netlify (publicare), GitHub (versionare cod).

Datele utilizatorului (favorite, meal planner, calcule calorice) sunt salvate local prin **localStorage**, asigurând persistența la refresh.

---

## 2. Descrierea problemei

Utilizatorii care doresc să gătească nu au o soluție unificată care să le ofere simultan idei de rețete și informații nutriționale pentru ingredientele dorite. În plus, planificarea meselor săptămânale și calculul caloric al rețetelor proprii necesită navigarea între mai multe aplicații separate.

RecipeLab rezolvă această problemă agregând date din două surse cloud independente într-o singură interfață simplă, oferind:

- Căutare rețete după ingredient
- Informații nutriționale pentru ingredientul căutat
- Salvarea rețetelor favorite
- Planificator săptămânal de mese (Meal Planner)
- Calculator de calorii cu salvarea calculelor

---

## 3. Descrierea API-urilor

### API 1 — TheMealDB
- **URL:** `https://www.themealdb.com/api/json/v1/1/`
- **Scop:** furnizează rețete, ingrediente, instrucțiuni și imagini
- **Autentificare:** nu este necesară (API public gratuit)
- **Endpoints utilizate:**
  - `filter.php?i={ingredient}` — caută rețete după ingredient
  - `lookup.php?i={id}` — obține detaliile complete ale unei rețete
  - `random.php` — returnează o rețetă aleatorie (folosită pentru "Recipe of the Day")

### API 2 — Open Food Facts
- **URL:** `https://world.openfoodfacts.org/cgi/search.pl`
- **Scop:** furnizează informații nutriționale (calorii, proteine, grăsimi, carbohidrați, fibre) per 100g
- **Autentificare:** nu este necesară (API public gratuit)
- **Utilizare:** atât în pagina principală (la căutarea unui ingredient) cât și în Calorie Calculator

---

## 4. Flux de date

### Metode HTTP utilizate
Ambele API-uri folosesc exclusiv metoda **GET**.

### Flux general al aplicației
Utilizator introduce ingredient
↓
Cerere simultană (Promise.all) către:
├── TheMealDB API → returnează lista de rețete
└── Open Food Facts API → returnează info nutriționale
↓
Rezultatele sunt afișate în interfață:
├── Carduri cu rețete (TheMealDB)
└── Tabel nutrițional (Open Food Facts)
↓
Utilizatorul poate:
├── Salva rețete la Favorite (localStorage)
├── Adăuga rețete în Meal Planner (localStorage)
└── Folosi Calorie Calculator (Open Food Facts + localStorage)

### Exemplu request / response — TheMealDB (căutare după ingredient)

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

### Exemplu request / response — TheMealDB (detalii rețetă)

**Request:**
GET https://www.themealdb.com/api/json/v1/1/lookup.php?i=52772

**Response:**
```json
{
  "meals": [
    {
      "idMeal": "52772",
      "strMeal": "Chicken Alfredo",
      "strCategory": "Chicken",
      "strArea": "Italian",
      "strInstructions": "...",
      "strIngredient1": "Chicken",
      "strMeasure1": "2 breasts",
      "strYoutube": "https://www.youtube.com/watch?v=..."
    }
  ]
}
```

### Exemplu request / response — TheMealDB (rețetă aleatorie)

**Request:**
GET https://www.themealdb.com/api/json/v1/1/random.php

**Response:** același format ca lookup, pentru o rețetă aleatorie — folosit pentru secțiunea "Recipe of the Day".

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
        "carbohydrates_100g": 0.0,
        "fiber_100g": 0.0
      }
    }
  ]
}
```

### Autentificare și autorizare
Niciun serviciu nu necesită autentificare. Ambele sunt API-uri publice, accesibile direct prin HTTP GET fără token sau cheie API.

### Persistență date
Datele utilizatorului sunt salvate în **localStorage** al browserului, asigurând persistența la refresh:
- `recipelab-favs` — rețetele salvate la favorite
- `recipelab-planner` — meal planner-ul săptămânal
- `recipelab-calcs` — calculele calorice salvate

---

## 5. Funcționalități aplicație

- **Recipe of the Day** — la fiecare încărcare a paginii apare o rețetă aleatorie în banner
- **Căutare rețete** — după ingredient, cu quick chips pentru Chicken, Salmon, Beef, Egg
- **Informații nutriționale** — afișate automat la fiecare căutare (calorii, proteine, grăsimi, carbohidrați, fibre)
- **Detalii rețetă** — modal cu ingrediente și instrucțiuni la click pe orice card
- **Favorite** — salvarea rețetelor preferate, vizibile în tab-ul Favourites
- **Share rețetă** — copierea linkului rețetei în clipboard
- **Meal Planner** — adăugarea rețetelor în zilele săptămânii (Monday–Sunday)
- **Calorie Calculator** — calculator caloric cu căutare ingredient via Open Food Facts, setare grame, listă ingrediente cu total calorii și salvare calcul cu nume personalizat
- **Saved Calculations** — istoricul calculelor calorice salvate, cu posibilitate de ștergere

---

## 6. Referințe

- TheMealDB API: https://www.themealdb.com/api.php
- Open Food Facts API: https://world.openfoodfacts.org/data
