# data_labelling
The code used to label incoming properties
🏡 Doma Property Image Tagging

Author: Daniel Accetta
Created: August 3, 2024
Last Revised: Sept 1, 2025
Status: ✅ Finalized

📌 Overview

This notebook applies AI-powered tagging to home listing images to support Doma Search and Recommendations. The tags enrich property listings with structured metadata that can be used for:

1. Improved property discovery
2. Personalized search results
3. Enhanced recommendations

The pipeline uses Google Gemini via the google-generativeai library, combined with structured property data, to generate descriptions, styles, and insights for each listing.

⚙️ Workflow
1. Data Loading
- Reads property data from a .csv file stored in Google Drive.
- Extracts image URLs and listing metadata (bedrooms, bathrooms, location, etc.).

2. Data Preprocessing
- Creates structured DataFrames:
-- df_listing_images: Extracts up to 11 property images per listing.
-0 df: Cleans and standardizes property metadata.

3. Prompt Engineering & AI Tagging
- Loads prompts from prompt.txt.
- Calls Gemini 1.5 Flash to generate tags, styles, names, and insights.
- Random values for recommendations are initially included as placeholders.

4. Exporting Results
- Saves processed DataFrames to Google Drive:
- response.csv → tagged listings with AI-generated fields
- response_images.csv → structured listing images

📂 Inputs & Outputs

Input:
- properties.csv (or custom dataset) from Google Drive
- Prompt file: prompt.txt

Output:
- response_3.csv (tagged property data)
- response_images_3.csv (image references)

🛠️ Requirements

The notebook requires the following dependencies:
- pip install -U google-generativeai keras tqdm opencv-python pillow pandas numpy

Python Libraries Used:
- google-generativeai (Gemini API)
- pandas, numpy, re, os, tqdm, random
- keras (neural network utilities)
- PIL, cv2, glob (image processing)
- google.colab utilities for Drive + Secrets

🔑 Setup

Store your Google API Key in Colab Secrets:

- from google.colab import userdata
- api_key = userdata.get('GOOGLE_API_KEY')
- os.environ['GOOGLE_API_KEY'] = api_key


Mount Google Drive:

- from google.colab import drive
- drive.mount('/content/drive', force_remount=True)


Ensure your dataset and prompt file are uploaded to the correct path.

🚀 Usage

- Open the notebook in Google Colab.
- Run the cells step by step.
- The model will process each listing and append AI-generated tags.
- Download final CSVs from Google Drive for use in Doma Search.

📊 Example Output Samples

Here’s a preview of what the final tagged dataset (response_3.csv) may look like:

id	propertyType	bedrooms	bathrooms	city	state	price	name	style	pitch	insight	recommendation	propertyImage
100245	House	3	2	Boston	MA	750000	Charming Cape Cod Home	Coastal	“Bright, open living with water views – perfect for New England life”	Great natural light in living rm	87	https://images.server.com/house123/front.jpg

100587	Condo	2	2	San Diego	CA	950000	Modern Beach Retreat	Contemporary	“Steps from the ocean with sleek design and private balcony”	Ideal for remote workers	92	https://images.server.com/condo567/kitchen.jpg

101112	Townhouse	4	3	Austin	TX	620000	Family Townhome Oasis	Traditional	“Spacious 4BR townhome with backyard and community amenities”	Great school district	74	https://images.server.com/townhome1112/exterior.jpg

And the image reference file (response_images_3.csv) may look like:

id	image 1	image 2	image 3
100245	https://images.server.com/house123/front.jpg
	https://images.server.com/house123/living.jpg
	https://images.server.com/house123/kitchen.jpg

100587	https://images.server.com/condo567/exterior.jpg
	https://images.server.com/condo567/living.jpg
	https://images.server.com/condo567/bedroom.jpg

101112	https://images.server.com/townhome1112/front.jpg
	https://images.server.com/townhome1112/backyard.jpg
	https://images.server.com/townhome1112/living.jpg
 
📌 Notes

- Current implementation applies Gemini tagging row by row — optimization opportunities exist for batch processing.
- Recommendation scores are randomly generated and should be replaced with a trained recommendation model.
- Ensure the dataset schema matches expected fields (e.g., imageURLs, propertyType, numBedroom).

📜 License

This project is developed as part of the Doma AI Search Initiative.
Usage is restricted to internal experimentation unless otherwise permitted.
