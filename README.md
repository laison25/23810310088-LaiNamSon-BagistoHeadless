# 23810310088-LaiNamSon-BagistoHeadless
23810310088-LaiNamSon-BagistoHeadless

# Báo Cáo Thực Hành - Bagisto Headless Commerce

**Họ và tên:** Lại Nam Sơn  
**MSSV:** 23810310088   
---

## Phần 1: Cài đặt Hệ thống

### Cài đặt Bagisto
- Cài đặt Bagisto 2.x qua Composer: `composer create-project bagisto/bagisto:^2.1 bagisto`
- Cấu hình `.env`, tạo database `bagisto`, chạy migration và seed
- Cài đặt GraphQL API: `composer require bagisto/graphql-api`
- Cài đặt GDPR package: `composer require bagisto/bagisto-gdpr`
- Tạo JWT secret: `php artisan jwt:secret`

### Danh sách 3 sản phẩm trong Admin Panel

<img width="1074" height="798" alt="Ảnh chụp màn hình 2026-04-03 151949" src="https://github.com/user-attachments/assets/29983182-7ef6-41e5-b337-d0a14afb149e" />


## Phần 2: Khai thác GraphQL API

### Query 1 – Lấy danh sách Categories

```graphql
{
  categories {
    data {
      id
      name
      slug
    }
  }
}
```

**Kết quả:**

<img width="1859" height="956" alt="Ảnh chụp màn hình 2026-04-03 153727" src="https://github.com/user-attachments/assets/c5dcf5fe-aa42-43fe-ac02-ff1fd29f9e8e" />


### Query 2 – Lấy 5 sản phẩm mới nhất

```graphql
{
  products(input: { page: 1, limit: 5 }) {
    data {
      id
      name
      price
      description
      urlKey
    }
  }
}
```

**Kết quả:**

<img width="1903" height="989" alt="Ảnh chụp màn hình 2026-04-03 160241" src="https://github.com/user-attachments/assets/021896b2-e83b-4821-894f-afa27712f254" />


### Query 3 – Lọc sản phẩm theo tên sinh viên

```graphql
{
  products(input: { name: "LaiNamSon", page: 1, limit: 5 }) {
    data {
      id
      name
      price
      description
      urlKey
    }
  }
}
```

**Kết quả (kèm Console log):**

<img width="1910" height="1035" alt="Ảnh chụp màn hình 2026-04-03 154445" src="https://github.com/user-attachments/assets/d8d7c67c-9b01-48c8-9b26-312de12a2e22" />


## Phần 3: Frontend hiển thị sản phẩm

Trang web được xây dựng bằng HTML/JavaScript (Fetch API), gọi GraphQL để hiển thị danh sách sản phẩm dạng Card.

**Ảnh minh chứng:**

<img width="1072" height="668" alt="Ảnh chụp màn hình 2026-04-03 160432" src="https://github.com/user-attachments/assets/c6b42200-dec4-42cd-9334-fff0b0882bdd" />


## IV. Câu hỏi bắt buộc

### Câu 1: So sánh Payload giữa REST API và GraphQL

Khi dùng REST API, server trả về toàn bộ object sản phẩm với đầy đủ các trường có trong database (có thể lên đến 30–50 trường như SKU, meta_title, meta_description, created_at, updated_at, images, variants...), dù client chỉ cần 5 trường. Điều này gây lãng phí băng thông đáng kể.

Trong bài thực hành, khi dùng GraphQL, em chỉ yêu cầu đúng 5 trường cần thiết (`id`, `name`, `price`, `description`, `urlKey`). Server chỉ trả về đúng dữ liệu được yêu cầu, giúp giảm payload xuống đáng kể so với REST, tăng tốc độ tải trang và tiết kiệm băng thông cho client.

---

### Câu 2: Query hay Mutation để thay đổi giá sản phẩm?

Để thay đổi giá của một sản phẩm, em sẽ dùng **Mutation**.

Trong GraphQL, **Query** chỉ được dùng để **đọc dữ liệu** (read-only), không làm thay đổi trạng thái hệ thống. Còn **Mutation** được dùng cho các thao tác **ghi, cập nhật hoặc xóa dữ liệu** trên server. Vì thay đổi giá là hành động cập nhật dữ liệu trong database, nên bắt buộc phải dùng Mutation. Ví dụ: `mutation { updateProduct(id: 1, input: { price: 299000 }) { id name price } }`.
