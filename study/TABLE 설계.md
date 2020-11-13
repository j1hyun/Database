# TABLE 설계

## 1. building

| id          | VARCHAR(5)  | PK       |
| ----------- | ----------- | -------- |
| name        | VARCHAR(32) |          |
| type        | VARCHAR(10) |          |
| latitude    | VARCHAR(32) |          |
| longitude   | VARCHAR(32) |          |
| detail_addr | VARCHAR(32) |          |
| code        | VARCHAR(5)  | FK(city) |

## 2. city

| code | VARCHAR(5)  | PK   |
| ---- | ----------- | ---- |
| name | VARCHAR(32) |      |

## 3. Measure_power

| date          | Date       |              |
| ------------- | ---------- | ------------ |
| time          | TIME       |              |
| id            | VARCHAR(5) | FK(building) |
| measure_power | FLOAT      |              |

## 4. predict_power

| date          | Date       |             |
| ------------- | ---------- | ----------- |
| time          | TIME       |             |
| id            | VARCHAR(5) | FK(bulding) |
| predict_power | FLOAT      |             |

## 5. measure_temp

| date         | DATE       |          |
| ------------ | ---------- | -------- |
| time         | TIME       |          |
| code         | VARCHAR(5) | FK(city) |
| measure_temp | FLOAT      |          |

## 6. predict_temp

| date         | DATE       |          |
| ------------ | ---------- | -------- |
| time         | TIME       |          |
| code         | VARCHAR(5) | FK(city) |
| predict_temp | FLOAT      |          |