#request beautiful soup pandas
import requests
from bs4 import BeautifulSoup

#this is as a travel article page
url = "https://www.example.com/top-20-travel-destinations"
headers = {
    "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64)"
}

#this part is for requesting page
response = requests.get(url, headers=headers)
soup = BeautifulSoup(response.content, "html5lib")

#results container
countries_data = []
# here: get top 20 countries , will update based on our travel web page
countries = soup.find_all("h2")[:20]

for i,country in enumerate(countries, 1):
    name = country.text.strip
    #find nearby paragraph
    desc_tag = country.find_next("p")
    description = desc_tag.text.strip() if desc_tag else "no description found."

    #get up to 3 nearby images
    image = []
    for img_tag in country.find_all_next("img", limit=3):
        src = img_tag.get("src")
        if src and "http" in src:
            image.append(src)
            if len(image) == 3:
                break

    countries_data.append({
        "name" : name,
        "description": description,
        "images": image


    })

#print summary
for country in countries_data:
    print(f"\n {country["name"]}")
    print(f"\n {country["description"]}")
    for img in country["image"]:
        print(f"{img}")


--->
