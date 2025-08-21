# 🌦️ **Power BI Weather Forecasting Dashboard**

> _“Plan smarter, stay informed, and make your day weather-ready!”_  

An **interactive, visually appealing Power BI dashboard** designed to help users track **weather trends, air quality, and upcoming forecasts** with a **modern UI/UX experience**.  

---

## 🎯 **Project Overview**

This dashboard empowers users to:  
- Track **weather predictions** for upcoming days 🌤️  
- Check **chance of rain** to plan events better ☔  
- Monitor **Air Quality Index (AQI)** across 6 cities 🌍  
- View **sunrise & sunset times** 🌅🌇  
- Analyze **average temperature trends** 📊  
- Access **real-time data** via integrated weather API  

> Designed with **orange & black theme visuals** for clarity, readability, and professional appeal.  

---

## ✨ **Key Features**

- **Interactive Forecasting & Current Weather Data**  
- **Cleaned and Preprocessed Datasets** using Power Query  
- **Custom DAX Measures**:  
  - Complex AQI formulas → simplified color-coded visuals 🌈  
  - Max/Min temperature tracking  
- **UI/UX Optimized Design** with responsive elements  
- **Real-time API Integration** for live updates  
- **Graphical Visualizations** for trends, alerts, and city-wise insights  

---

## 🛠️ **Tech Stack**

- **Power BI Desktop & Service**  
- **DAX (Data Analysis Expressions)** for custom metrics  
- **Real-time Weather API** integration  
- **Data Cleaning, Preprocessing, Feature Scaling**  
- **UI/UX Design** for dashboard responsiveness & clarity  

---

## 📊 **Data Sources**

- Forecasting dataset for upcoming days  
- Current weather dataset (6 cities)  
- AQI dataset (Air Quality Index)  
- Sunrise & sunset timings dataset  
- Real-time API for live updates  

---

### **Dashboard Preview**

<img width="1292" height="727" alt="Image" src="https://github.com/user-attachments/assets/cf76e285-fad8-47fb-8697-660f086394b5" />

*Interactive orange & black themed dashboard showing weather, AQI, and trends.*

## Color-Coded AQI DAX code

<img width="653" height="287" alt="Image" src="https://github.com/user-attachments/assets/3017a92a-78f0-448e-b787-66636c5c4e9e" />

*Color-coded AQI metrics for multiple cities.*

## Measures Created

<img width="403" height="757" alt="Image" src="https://github.com/user-attachments/assets/a5a47bd6-a448-403c-a869-dfc396ea6ef3" />


*Average temperature , Different measures like AQIs, and other important measures for easy interpretation and calculations.*


## 🌬️ AQI (CPCB) Calculation in Power BI

The **Air Quality Index (AQI)** is calculated using **PM2.5, PM10, O3, NO2, SO2, and CO** values based on CPCB standards.  
The DAX formula computes sub-indices for each pollutant and returns the **maximum sub-index** as the final AQI.

```DAX
AQI (CPCB) Value = 

VAR PM25 = SELECTEDVALUE('Current'[current.air_quality.pm2_5])
VAR PM10 = SELECTEDVALUE('Current'[current.air_quality.pm10])
VAR O3   = SELECTEDVALUE('Current'[current.air_quality.o3])
VAR NO2  = SELECTEDVALUE('Current'[current.air_quality.no2])
VAR SO2  = SELECTEDVALUE('Current'[current.air_quality.so2])
VAR COraw= SELECTEDVALUE('Current'[current.air_quality.co])

VAR COmg = 
    IF (
        ISBLANK(COraw),
        BLANK(),
        IF ( COraw > 100, COraw / 1000.0, COraw )
    )

VAR SI_PM25 =
    SWITCH(
        TRUE(),
        ISBLANK(PM25), BLANK(),
        PM25 <= 30,   (50 - 0)   / (30 - 0)    * (PM25 - 0)    + 0,
        PM25 <= 60,   (100 - 51) / (60 - 31)   * (PM25 - 31)   + 51,
        PM25 <= 90,   (200 - 101)/ (90 - 61)   * (PM25 - 61)   + 101,
        PM25 <= 120,  (300 - 201)/ (120 - 91)  * (PM25 - 91)   + 201,
        PM25 <= 250,  (400 - 301)/ (250 - 121) * (PM25 - 121)  + 301,
        500
    )

VAR SI_PM10 =
    SWITCH(
        TRUE(),
        ISBLANK(PM10), BLANK(),
        PM10 <= 50,   (50 - 0)   / (50 - 0)    * (PM10 - 0)    + 0,
        PM10 <= 100,  (100 - 51) / (100 - 51)  * (PM10 - 51)   + 51,
        PM10 <= 250,  (200 - 101)/ (250 - 101) * (PM10 - 101)  + 101,
        PM10 <= 350,  (300 - 201)/ (350 - 251) * (PM10 - 251)  + 201,
        PM10 <= 430,  (400 - 301)/ (430 - 351) * (PM10 - 351)  + 301,
        500
    )

VAR SI_O3 =
    SWITCH(
        TRUE(),
        ISBLANK(O3), BLANK(),
        O3 <= 50,    (50 - 0)   / (50 - 0)    * (O3 - 0)     + 0,
        O3 <= 100,   (100 - 51) / (100 - 51)  * (O3 - 51)    + 51,
        O3 <= 168,   (200 - 101)/ (168 - 101) * (O3 - 101)   + 101,
        O3 <= 208,   (300 - 201)/ (208 - 169) * (O3 - 169)   + 201,
        O3 <= 748,   (400 - 301)/ (748 - 209) * (O3 - 209)   + 301,
        500
    )

VAR SI_NO2 =
    SWITCH(
        TRUE(),
        ISBLANK(NO2), BLANK(),
        NO2 <= 40,   (50 - 0)   / (40 - 0)    * (NO2 - 0)    + 0,
        NO2 <= 80,   (100 - 51) / (80 - 41)   * (NO2 - 41)   + 51,
        NO2 <= 180,  (200 - 101)/ (180 - 81)  * (NO2 - 81)   + 101,
        NO2 <= 280,  (300 - 201)/ (280 - 181) * (NO2 - 181)  + 201,
        NO2 <= 400,  (400 - 301)/ (400 - 281) * (NO2 - 281)  + 301,
        500
    )

VAR SI_SO2 =
    SWITCH(
        TRUE(),
        ISBLANK(SO2), BLANK(),
        SO2 <= 40,    (50 - 0)   / (40 - 0)     * (SO2 - 0)     + 0,
        SO2 <= 80,    (100 - 51) / (80 - 41)    * (SO2 - 41)    + 51,
        SO2 <= 380,   (200 - 101)/ (380 - 81)   * (SO2 - 81)    + 101,
        SO2 <= 800,   (300 - 201)/ (800 - 381)  * (SO2 - 381)   + 201,
        SO2 <= 1600,  (400 - 301)/ (1600 - 801) * (SO2 - 801)   + 301,
        500
    )

VAR SI_CO =
    SWITCH(
        TRUE(),
        ISBLANK(COmg), BLANK(),
        COmg <= 1.0,  (50 - 0)   / (1.0 - 0.0)  * (COmg - 0.0)  + 0,
        COmg <= 2.0,  (100 - 51) / (2.0 - 1.1)  * (COmg - 1.1)  + 51,
        COmg <= 10.0, (200 - 101)/ (10.0 - 2.1) * (COmg - 2.1)  + 101,
        COmg <= 17.0, (300 - 201)/ (17.0 - 10.0)* (COmg - 10.0) + 201,
        COmg <= 34.0, (400 - 301)/ (34.0 - 17.0)* (COmg - 17.0) + 301,
        500
    )

VAR AQI_Final =
    MAXX(
        {
            SI_PM25,
            SI_PM10,
            SI_O3,
            SI_NO2,
            SI_SO2,
            SI_CO
        },
        [Value]
    )

RETURN
    IF ( ISBLANK(AQI_Final), BLANK(), ROUND(AQI_Final, 0) )
```


---

## 🚀 **How to Use**

1. Open the **Power BI dashboard** in **Power BI Service**.  
2. Use interactive **filters** to select cities or forecast dates.  
3. View **weather trends, AQI levels, and planning insights**.  
4. **Real-time updates** ensure accurate, live data.  

---

## 🔗 **Project Links**

- **Live Power BI Dashboard:**  https://app.powerbi.com/groups/me/reports/4f9003b0-b010-471e-99bc-ea9b432c312e?ctid=34bd8bed-2ac1-41ae-9f08-4e0a3f11706c&pbi_source=linkShare  

---

## 👨‍💻 **Author**

**Mihir Kumar** – Data & Visualization Enthusiast  

Connect on [LinkedIn](https://www.linkedin.com/in/mihir-singh-0974832a8/)  

---

## ⚡ **License**

MIT License  
