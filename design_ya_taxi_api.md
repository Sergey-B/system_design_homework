## **Спроектировать API для Яндекс Такси (REST / gRPC / со звездочкой GraphQL):**

*   рассчитать время и стоимость маршрута (со звездочкой еще и разные тарифы);
*   сделать заказ такси по выбранному маршруту (со звездочкой еще и разные пути);
*   посмотреть профиль водителя и клиента 
*   узнать статус загруженности водителей
*   посмотреть историю поездок
*   оставить отзыв о поездке
*   изменить свои данные  
     

  
**Рассчитать время и стоимость маршрута**

```javascript
// Request

POST /taxi_rides/calculate
Content-Type: application/json

{
  from_address: string,
  to_address: string,
  client_profile_id: GUID,
  time_period: [from_time, to_time]
}
```

```javascript
// Response

{
  drive_route: [     
       part: {
         from: [longitude, latitude],
         to: [longitude, latitude],
         estimated_time: int
       }
       …
  ],
  available_drivers: [
     { profile_id: GUID, coordinates: [longitude, latitude], }
  ],
  estimated_cost: int
}
```

**Cделать заказ такси по выбранному маршруту**

```javascript
POST /taxi_rides
Content-Type: application/json

{
	client_profile_id: GUID,
	from_address: string,
	to_address: string,
	cost: int,
	timestamp: string
}
```

```javascript
  Response

  {
    id: GUID,
    driver_profile: {
        id: GUID,
        profile_picture_url: string,
        rating: float,
    first_name: string,
    last_name: string,
    car_description: string,
     },
    car_position: [longitude, latitude],
    driver_to_client_route: [
       part: {
         from: [longitude, latitude],
         to: [longitude, latitude],
         estimated_time: int
       }
       …
    ],
    from_to_address_route: [     
       part: {
         from: [longitude, latitude],
         to: [longitude, latitude],
         estimated_time: int
       }
       …
    ],
    estimated_cost: int
  }
```

**Узнать статус загруженности водителей**

```javascript
GET /clients_drivers_ratio?long=int&latitude=int&time_period=[from_time, to_time]

{
   ratio: low|medium|high,
   increase_cost_coefficient: int
}
```

**Посмотреть профиль водителя и клиента**

```javascript
// Посмотреть профиль водителя
GET /users/{:user_id}/driver_profile

// Response
{
  id: GUID,
  first_name: string,
  last_name: string,
  phone_number: string,
  car_description: string,
  profile_picture: string,
  rating: number
}
```

```javascript
// Посмотреть профиль клиента
GET /users/{:user_id}/client_profile

// Response
{
  id: GUID,
  first_name: string,
  last_name: string,
  phone_number: string,
  profile_picture: string,
  rating: number
}
```

**Посмотреть историю поездок**

```javascript
GET /taxi_rides?limit=int&offset=int&client_id=int

// Response
{
  total: int,
  taxi_rides: [
  id: GUID,
  start_time: datetime,
  cost: number,
  from_addres: string,
  to_address: string,
  ride_time: int,
        driver_profile: {
          id: GUID,
          profile_picture_url: string,
          rating: float,
      first_name: string,
      last_name: string,
      car_description: string,
     },
        route_picture_url: string,
        client_id: GUID,
  payment_method_id: GUID
  ]
}
```

**Оставить отзыв о поездке**

```javascript
POST /taxi_rides/{:ride_id}/reviews

// Response
{
  id: GUID,
  timestamp: datetime,
  client_id: GUID,
  description: string,
  was_comfortable: bool,
  grade: int
}
```

**Изменить свои данные**

```javascript
// Изменить профиль клиента

PUT|PATCH /users/{:user_id}/client_profile

// Response
{
  id: GUID,
  first_name: string,
  last_name: string,
  phone_number: string,
  payment_method_id: GUID
}
```

```javascript
// Изменить профиль водителя

PUT|PATCH /users/{:user_id}/driver_profile

// Response
{
  id: GUID,
  first_name: string,
  last_name: string,
  phone_number: string,
  profile_picture: string,
  car_description: string
}
```