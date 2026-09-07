# StoreApiLR11

ASP.NET Core Web API с Entity Framework Core, MySQL и обработкой ошибок в формате Problem Details.

## Запуск

Создайте базу данных `store_db`, при необходимости измените строку подключения в `appsettings.json`, затем выполните:

```bash
dotnet ef migrations add InitialCreate
dotnet ef database update
dotnet run
```

Swagger: `https://localhost:7193/swagger`.

## Проверка обработки ошибок

| URL | Метод | Условие | Ожидаемый результат |
|---|---|---|---|
| `/api/Products` | POST | Пустое имя и цена `-10` | `400 Bad Request`, ошибки валидации |
| `/api/Orders` | POST | `customerId: 99999` | `404 Not Found`, клиент не найден |
| `/api/Orders` | POST | Количество товара `9999` | `400 Bad Request`, недостаточный остаток |
| `/api/Categories/1` | DELETE | У категории есть товары | `409 Conflict`, нарушение ссылочной целостности |
| `/api/test-crash` | GET | Искусственное исключение | `500 Internal Server Error`, ответ `ProblemDetails` |

В среде `Development` глобальный обработчик включает текст исключения. В остальных средах возвращается безопасное общее описание.
