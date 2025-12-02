# API Jadwal Sholat Kemenag
A RESTful API for retrieving prayer times in Indonesia. This API provides prayer schedules for different cities and provinces. All prayer time data is sourced directly from Kemenag RI website.

## Table of Contents
- [Features](#features)
- [Base URL](#base-url)
- [API Endpoints](#api-endpoints)
- [Usage Examples](#usage-examples)
- [Error Handling](#error-handling)

## Features
- Fetch prayer times for specific cities and dates
- Fetch prayer schedules for multiple days (up to 10)
- List all provinces in Indonesia
- List cities by province
- Prayer time data from Kemenag RI
- Automatic data synchronization with Kemenag RI website

## Base URL
```
https://jadwal-sholat.terusberbuatbaik.com
```

## API Endpoints

### Get All Provinces
```http
GET /provinces
```
Response:
```json
{
  "data": [
    {
      "id": "11",
      "label": "ACEH"
    },
    // ...more provinces
  ]
}
```

### Get Cities by Province
```http
GET /cities?province_id=11
```
Response:
```json
{
  "data": [
    {
      "id": "1101",
      "label": "KAB. SIMEULUE"
    },
    // ...more cities
  ]
}
```

### Get Prayer Schedule
```http
GET /schedules?city_id=1101&date=2024-03-20&days=5
```
Response:
```json
{
  "data": [
    {
      "date": "2024-03-20",
      "cityId": "1101",
      "imsak": "05:01",
      "subuh": "05:11",
      "terbit": "06:20",
      "dhuha": "06:47",
      "dzuhur": "12:27",
      "ashar": "15:31",
      "maghrib": "18:31",
      "isya": "19:39"
    },
    {
      "date": "2024-03-21",
      "cityId": "1101",
      "imsak": "05:02",
      "subuh": "05:12",
      "terbit": "06:21",
      "dhuha": "06:48",
      "dzuhur": "12:28",
      "ashar": "15:32",
      "maghrib": "18:32",
      "isya": "19:40"
    },
    // ...more schedules for the next 3 days
  ]
}
```

## Usage Examples

### Using Fetch API
```typescript
// Fetch provinces
const getProvinces = async () => {
  const response = await fetch(
    "https://jadwal-sholat.terusberbuatbaik.com/provinces"
  );
  const data = await response.json();
  return data;
};

// Fetch cities by province
const getCities = async (provinceId: string) => {
  const response = await fetch(
    `https://jadwal-sholat.terusberbuatbaik.com/cities?province_id=${provinceId}`
  );
  const data = await response.json();
  return data;
};

// Fetch prayer schedule
const getPrayerSchedule = async (cityId: string, date: string, days: string) => {
  const response = await fetch(
    `https://jadwal-sholat.terusberbuatbaik.com/schedules?city_id=${cityId}&date=${date}&days=${days}`
  );
  const data = await response.json();
  return data;
};
```

### Using Axios
```typescript
import axios from "axios";

// Fetch provinces
const getProvinces = async () => {
  const { data } = await axios.get(
    "https://jadwal-sholat.terusberbuatbaik.com/provinces"
  );
  return data;
};

// Fetch cities by province
const getCities = async (provinceId: string) => {
  const { data } = await axios.get(
    `https://jadwal-sholat.terusberbuatbaik.com/cities`,
    {
      params: { province_id: provinceId },
    }
  );
  return data;
};

// Fetch prayer schedule
const getPrayerSchedule = async (cityId: string, date: string, days: string) => {
  const { data } = await axios.get(
    `https://jadwal-sholat.terusberbuatbaik.com/schedules`,
    {
      params: {
        city_id: cityId,
        date: date,
        days: days,
      },
    }
  );
  return data;
};
```

## Error Handling
The API returns appropriate error status code and messages in the following format:
```json
{
  "error": "Error message here"
}
```

Common error cases:
- Invalid province ID (404)
- Invalid city ID (404)
- Invalid date format (422)
- Invalid number of days (422)
- Failed to fetch prayer schedules from source (400)

## Data Source
All prayer time data is sourced from the official Kemenag RI website. The schedules are automatically synchronized with Kemenag's system.
