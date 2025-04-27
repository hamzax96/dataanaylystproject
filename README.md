# dataanaylystproject
#Data cleaning and Visualization 
import pandas as pd

file_path = "/content/My data.csv"
df = pd.read_csv("/content/My data.csv")
df = df.drop(columns=[col for col in df.columns if 'Unnamed' in col], errors='ignore')
df.columns = df.columns.str.strip()
df_pakistan = df[df['country'].str.strip().str.lower() == 'pakistan'].copy()

city_state_corrections = {
    "karachi": "Sindh",
    "lahore": "Punjab",
    "islamabad": "Islamabad Capital Territory",
    "rawalpindi": "Punjab",
    "peshawar": "Khyber Pakhtunkhwa",
    "quetta": "Balochistan",
    "faisalabad": "Punjab",
    "multan": "Punjab",
    "sialkot": "Punjab",
    "hyderabad": "Sindh"
}

df_pakistan['city'] = df_pakistan['city'].fillna('').str.strip().str.lower()
df_pakistan['state'] = df_pakistan.apply(lambda row: city_state_corrections.get(row['city'], row['state']), axis=1)
df_pakistan['state'] = df_pakistan['state'].fillna('Unknown')
df_pakistan.to_csv("/content/My data.csv", index=False)

print("Data cleaning for Pakistan completed and saved.")

