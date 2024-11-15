
# **Football Squad Builder**

## **Description**

Le **Football Squad Builder** est un projet innovant qui combine **web scraping**, **analyse de données**, et **simulation dans Football Manager 2024 (FM24)**.  
L'objectif est d'explorer l'impact des données dans la construction d'une équipe de football performante, en intégrant des joueurs sélectionnés sur la base de leur potentiel et de leur valeur de marché.

### Ce projet inclut :
- **Scraping** des 500 joueurs les plus chers et des 500 jeunes prometteurs (< 21 ans) depuis Transfermarkt.
- **Simulation** d'une saison complète dans FM24 avec un club personnalisé, en analysant les performances des joueurs et les stratégies.
- **Développement** d'un site web permettant aux utilisateurs de générer une équipe en fonction d'un budget et d'une formation tactique.

---

## **Déroulement du projet**

### **Saison dans FM24**
1. **Création du club et début de saison** :  
   - Sélection initiale basée sur les données scrapées.  
   - Une première moitié de saison difficile, terminant à la 18ᵉ place en Premier League en janvier.  
   
2. **Recrutement intelligent** :  
   - Renforcement de l’équipe lors du mercato hivernal grâce à des recrues identifiées dans les données.  

3. **Résultats** :  
   - Une remontée spectaculaire à la 7ᵉ place en fin de saison, décrochant une qualification pour la Conference League.

### **Développement Web**
Actuellement, nous développons un site web qui permettra aux utilisateurs de :
- Entrer un **budget (en millions)**.
- Sélectionner une **formation tactique** (ex : 4-3-3, 3-5-2, etc.).
- Générer une équipe optimisée selon les données scrapées.

---

## **Fonctionnalités (Actuelles et Futures)**

- **Scraping des données** :
  - Extraction automatisée des statistiques des joueurs sur Transfermarkt, avec des informations comme :
    - Nom, poste, club, valeur marchande.
    - Statistiques détaillées (matchs joués, buts, passes décisives, etc.).

- **Simulation Football Manager 2024** :
  - Analyse des performances des joueurs en conditions réelles.
  - Ajustement tactique basé sur les résultats.

- **Génération d'équipes** (en cours) :
  - Création d'équipes optimisées en fonction des contraintes budgétaires et tactiques.

- **Développement web** (en cours) :
  - Interface intuitive pour démocratiser l’utilisation des données dans la gestion d’équipe de football.

---

## **Aperçu du Code**

Voici un extrait du script de scraping utilisé pour extraire les données de Transfermarkt :  

```python
import requests
from bs4 import BeautifulSoup
import time

class YoungPlayerStatsScraper:
    def __init__(self):
        self.headers = {
            'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/91.0.4472.124 Safari/537.36'
        }
        self.base_url = 'https://www.transfermarkt.com'

    def get_players_stats(self, base_url, total_pages=20):
        all_players_stats = []

        for page in range(1, total_pages + 1):
            url = base_url if page == 1 else f"{base_url}&page={page}"
            print(f"\nScraping page {page}/{total_pages}")
            
            response = requests.get(url, headers=self.headers)
            soup = BeautifulSoup(response.content, 'html.parser')
            
            players = soup.select('table.items > tbody > tr:not(.thead)')
            for player in players:
                try:
                    name_element = player.select_one('td.hauptlink a')
                    name = name_element.text.strip()
                    player_url = self.base_url + name_element['href']
                    value_element = player.select_one('td.rechts.hauptlink')
                    value = value_element.text.strip() if value_element else "Not specified"
                    
                    all_players_stats.append({
                        'name': name,
                        'market_value': value,
                        'profile_url': player_url,
                    })
                except Exception as e:
                    print(f"Error: {str(e)}")
            time.sleep(2)
        return all_players_stats

# Exemple d'utilisation
scraper = YoungPlayerStatsScraper()
data = scraper.get_players_stats('https://www.transfermarkt.com/spieler-statistik/wertvollstespieler/marktwertetop?altersklasse=u21', total_pages=5)
print(f"Joueurs extraits : {len(data)}")
```

---

## **Structure des fichiers**
- `young_players_stats.csv` : Contient les données structurées sur les jeunes joueurs.
- `young_players_stats.json` : Une version JSON des mêmes données, pour intégration dans d'autres applications.
- `app/` : Répertoire contenant le code pour l'interface web (en cours).

---

## **Prochaines Étapes**
- [x] Finaliser le scraping des données.
- [x] Développer les bases du modèle de génération d’équipes.
- [ ] Tester la génération d’équipes pour différents budgets et formations.
- [ ] Finaliser le design du site web.
- [ ] Déployer l’application sur **Vercel**.

