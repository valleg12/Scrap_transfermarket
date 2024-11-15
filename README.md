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

(requests pour récupérer les pages web/BeautifulSoup pour analyser le contenu HTML/pandas pour structurer et enregistrer les données)

class YoungPlayerStatsScraper:
    def __init__(self):
        self.headers = {
            'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/91.0.4472.124 Safari/537.36'
        }
        self.base_url = 'https://www.transfermarkt.com'

(Rôle : Configurer l’URL de base et les en-têtes HTTP pour éviter d’être bloqué par les serveurs de Transfermarkt/Explication :Les en-têtes (User-Agent) simulent un navigateur pour éviter d’être identifié comme un bot/Utilisation de la méthode self.base_url pour centraliser la gestion des URLs)

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

(Rôle : Récupérer les statistiques des joueurs sur plusieurs pages/Explication du choix des outils :La boucle for itère sur les pages pour garantir que toutes les données sont récupérées/La méthode requests.get est utilisée pour envoyer une requête HTTP et récupérer le contenu des pages/La bibliothèque BeautifulSoup analyse le HTML et permet d’extraire les éléments ciblés comme les noms, positions et statistiques/Utilisation de time.sleep : Permet d’éviter que le serveur ne détecte le scraper comme un bot en espaçant les requêtes)

# Exemple d'utilisation
scraper = YoungPlayerStatsScraper()
data = scraper.get_players_stats('https://www.transfermarkt.com/spieler-statistik/wertvollstespieler/marktwertetop?altersklasse=u21', total_pages=5)
print(f"Joueurs extraits : {len(data)}")
```

---

## **Structure des fichiers**
- `young_players_stats.csv` : Contient les données structurées sur les jeunes joueurs.
- `young_players_stats.json` : Une version JSON des mêmes données, pour intégration dans d'autres applications.

---

## **Prochaines Étapes**
- [x] Finaliser le scraping des données.
- [x] Développer les bases du modèle de génération d’équipes.
- [ ] Tester la génération d’équipes pour différents budgets et formations.
- [ ] Finaliser le design du site web.
- [ ] Déployer l’application sur **Vercel**.

---

Football Squad Builder

Description

The Football Squad Builder is an innovative project combining web scraping, data analysis, and simulation in Football Manager 2024 (FM24).
The goal is to explore the impact of data in building a successful football team by selecting players based on their potential and market value.

This project includes:

	•	Scraping the top 500 most valuable players and the top 500 promising young players (< 21 years old) from Transfermarkt.
	•	Simulating a full season in FM24 with a custom club, analyzing player performances and strategies.
	•	Developing a website that allows users to generate a squad based on a specified budget and tactical formation.

Project Workflow

FM24 Season Simulation

	1.	Club Creation and Season Start:
	•	Initial squad selected based on scraped data.
	•	A challenging first half of the season, finishing in 18th place in the Premier League by January.
	2.	Smart Recruitment:
	•	Strengthened the team during the winter transfer window using recruits identified from the data.
	3.	Results:
	•	A spectacular comeback to 7th place by the end of the season, securing qualification for the Conference League.

Web Development

We are currently developing a website that will allow users to:
	•	Enter a budget (in millions).
	•	Select a tactical formation (e.g., 4-3-3, 3-5-2, etc.).
	•	Generate an optimized squad based on scraped data.

Features (Current and Future)

	•	Data Scraping:
	•	Automated extraction of player statistics from Transfermarkt, including:
	•	Name, position, club, market value.
	•	Detailed stats (matches played, goals, assists, etc.).
	•	FM24 Simulation:
	•	Performance analysis of players in real-game conditions.
	•	Tactical adjustments based on results.
	•	Squad Generation (in progress):
	•	Creation of optimized squads based on budgetary and tactical constraints.
	•	Web Development (in progress):
	•	Intuitive interface to democratize the use of data in football team management.

Code Overview

Here is a snippet of the scraping script used to extract data from Transfermarkt:

import requests
from bs4 import BeautifulSoup
import time

(requests to fetch web pages / BeautifulSoup to parse HTML content / pandas to structure and save data)

class YoungPlayerStatsScraper:
    def __init__(self):
        self.headers = {
            'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/91.0.4472.124 Safari/537.36'
        }
        self.base_url = 'https://www.transfermarkt.com'

(Role: Configure the base URL and HTTP headers to avoid being blocked by Transfermarkt servers / Explanation: Headers (User-Agent) simulate a browser to prevent detection as a bot / Using `self.base_url` to centralize URL management)

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

(Role: Retrieve player stats across multiple pages / Explanation of tools: The `for` loop ensures all data is retrieved / `requests.get` sends an HTTP request to fetch page content / BeautifulSoup parses the HTML and allows targeted extraction of elements like names, positions, and stats / `time.sleep` avoids server detection as a bot by spacing out requests)

# Example usage
scraper = YoungPlayerStatsScraper()
data = scraper.get_players_stats('https://www.transfermarkt.com/spieler-statistik/wertvollstespieler/marktwertetop?altersklasse=u21', total_pages=5)
print(f"Players extracted: {len(data)}")

File Structure

	•	young_players_stats.csv: Contains structured data on young players.
	•	young_players_stats.json: A JSON version of the same data for integration into other applications.

Next Steps

	•	Finalize data scraping.
	•	Develop the foundational squad generation model.
	•	Test squad generation for various budgets and formations.
	•	Finalize website design.
	•	Deploy the application on Vercel.
