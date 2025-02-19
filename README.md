# Postman_Study_Notes
希望自己能组织好语言回答

---

### 1.你如何使用 Postman 进行接口测试？  

#### **场景描述：**  

在我参与的项目中，如 **书悦空间** 和 **食派校园** 项目，我经常使用 **Postman** 来进行接口测试，验证系统的后端服务是否按预期提供功能和正确处理数据。我负责通过接口进行数据验证、功能验证及性能验证，确保前端与后端的交互是稳定和准确的。

---

### **使用 Postman 进行接口测试的步骤：**

1. **接口功能测试：**

   在项目中，我们的系统涉及多个API接口，如用户注册、订单提交、商品查询等。我使用 **Postman** 进行接口功能测试，编写测试用例并模拟不同的请求，验证接口是否按预期返回正确的响应。

   - 例如，在 **食派校园** 项目中，我们有一个用于提交订单的接口，通过 POST 请求提交订单信息，我通过 Postman 测试接口的功能是否正常。
   
   ```bash
   POST https://api.foodorder.com/orders
   Content-Type: application/json
   {
       "userId": 123,
       "productId": 456,
       "quantity": 2
   }
   ```

   **步骤**：
   - 我通过 Postman 创建了一个新的请求，选择了 **POST** 方法，输入接口的 URL。
   - 在请求体中填入了 JSON 格式的数据，模拟了一个用户提交订单的场景。
   - 点击发送请求后，我查看了接口返回的响应，验证状态码是否是 `200 OK`，并检查返回的数据是否包含预期的内容，如订单号、产品信息等。

2. **接口响应验证：**

   每次接口测试之后，我会对接口返回的响应进行验证，确保它符合预期的结果。这包括状态码、返回的数据结构和内容。

   - **状态码**：比如 `200 OK` 表示请求成功，`400 Bad Request` 表示请求参数有误等。通过 Postman，我可以在响应部分查看返回的状态码，判断接口是否正常工作。
   
   - **返回数据**：我还会验证返回的 JSON 数据是否包含预期的字段和正确的值。例如，对于订单提交接口，我会验证返回的订单号、商品信息、总价等内容是否与请求内容一致。

   ```json
   {
       "status": "success",
       "orderId": "123456",
       "product": "Test Dish",
       "quantity": 2,
       "totalPrice": 20.0
   }
   ```

   - **自动化验证**：为了提高效率，我在 Postman 中设置了 **测试脚本**，自动化验证接口的响应。例如：
     ```javascript
     pm.test("Status code is 200", function () {
         pm.response.to.have.status(200);
     });
     pm.test("Order ID is returned", function () {
         pm.response.to.have.jsonBody('orderId');
     });
     pm.test("Total price is correct", function () {
         pm.response.to.have.jsonBody('totalPrice').that.equals(20);
     });
     ```

   这些测试脚本可以帮助我在接口请求后自动验证响应的状态码、数据内容是否符合预期，从而节省了手动验证的时间。

3. **接口性能和负载测试：**

   在某些情况下，接口的性能也是必须验证的，尤其是当系统需要处理大量请求时。在 **书悦空间** 项目中，我使用 Postman 配合其他工具进行性能测试，模拟大量并发请求，检查接口的响应时间和稳定性。

   通过 **Postman Runner**，我能够运行多个请求并设置执行次数，模拟接口在高并发情况下的表现，帮助团队发现性能瓶颈。

   - 例如，我可以设置 **Runner** 来执行多个订单提交请求，并查看每个请求的响应时间：
     ```bash
     POST https://api.foodorder.com/orders
     Content-Type: application/json
     {
         "userId": 123,
         "productId": 456,
         "quantity": 10
     }
     ```

   通过查看返回的响应时间，我能判断接口在不同负载下的表现，帮助团队进行优化。

4. **接口安全性测试：**

   在接口测试中，除了功能和性能外，安全性也是一个重要的测试点。在 **书悦空间** 项目中，我使用 **Postman** 测试了接口的权限控制和身份验证功能。

   - **JWT 验证**：例如，我通过 Postman 测试了接口的 **JWT（JSON Web Token）** 认证，确保只有合法用户才能访问某些敏感的接口。
     ```bash
     GET https://api.foodorder.com/orders/123456
     Authorization: Bearer <valid_jwt_token>
     ```

   - **无权限访问**：我还测试了当没有提供有效 Token 时，接口是否正确返回 **403 Forbidden** 错误。

5. **使用环境变量和全局变量：**

   在多个接口测试场景中，我使用了 **Postman 环境变量** 来保存不同环境下的 API URL、授权令牌等信息。例如，在开发环境和生产环境中，API 地址会有所不同，我通过设置环境变量来简化请求配置，避免手动修改每个请求的 URL 或 token。

   - 在 Postman 中，我设置了环境变量：
     ```json
     {
       "apiUrl": "https://api.foodorder.com",
       "authToken": "your_jwt_token"
     }
     ```

   - 在请求中直接使用这些变量：
     ```bash
     GET {{apiUrl}}/orders/123456
     Authorization: Bearer {{authToken}}
     ```

---

#### **总结：**

在项目中，我通过 **Postman** 进行接口测试，不仅验证了接口的功能是否正常，还通过设置自动化验证脚本提高了测试效率。此外，我还进行过接口的性能、负载和安全性测试，确保接口在高并发环境下稳定运行，并验证了权限控制和身份验证的正确性。

通过这种方式，Postman 帮助我全面、系统地验证了接口的各项功能，保证了系统的稳定性和数据的准确性。

---

### 2.描述一个你设计过的复杂的接口测试用例。

根据你的经历，以下是一个描述 **设计复杂接口测试用例** 的示例，结合你在项目中使用的工具和测试场景。

---

### **复杂接口测试用例：** 用户订单接口功能验证

#### **背景：**
在 **食派校园** 项目中，系统包含了一个用于用户提交订单的接口。当用户提交订单时，系统会根据用户的选择生成订单，并返回相应的订单信息。接口返回的数据必须包含用户的订单号、商品信息、数量、价格等。为了确保接口功能的正确性，我设计了一个 **订单提交接口** 的复杂测试用例。

#### **接口信息：**
- **接口 URL**：`POST https://api.foodorder.com/orders`
- **请求体**：
   ```json
   {
     "userId": 123,
     "productId": 456,
     "quantity": 2
   }
   ```
- **响应体**：
   ```json
   {
     "status": "success",
     "orderId": "123456",
     "product": "Test Dish",
     "quantity": 2,
     "totalPrice": 20.0
   }
   ```

---

### **设计复杂接口测试用例：**

#### **1. 正常情况（基本功能验证）**

**目标**：验证接口能正确处理有效的请求，并返回正确的订单信息。

- **输入数据**：  
  - `userId`: 123  
  - `productId`: 456  
  - `quantity`: 2

- **预期结果**：  
  - 状态码应为 `200 OK`  
  - 返回的 `status` 应为 `success`  
  - 返回的 `orderId` 应为一个有效的订单号  
  - `product` 应该是 "Test Dish"  
  - `quantity` 应该是 2  
  - `totalPrice` 应该是 20.0

#### **2. 边界测试（输入边界值）**

**目标**：验证接口如何处理极限输入数据，如最大或最小数量。

- **输入数据**：  
  - `userId`: 123  
  - `productId`: 456  
  - `quantity`: 0（最小值）  

- **预期结果**：  
  - 状态码应为 `400 Bad Request` 或 `422 Unprocessable Entity`  
  - 返回的错误消息应指示 `quantity` 无效

- **输入数据**：  
  - `quantity`: 10000（最大值）  

- **预期结果**：  
  - 返回的 `totalPrice` 应正确计算，并且系统应能够处理该数量而不崩溃。  

#### **3. 错误请求（无效数据测试）**

**目标**：验证接口如何处理无效的数据，确保系统能够优雅地返回错误信息。

- **输入数据**：  
  - `userId`: 99999（无效的用户ID）  
  - `productId`: 456  
  - `quantity`: 2

- **预期结果**：  
  - 状态码应为 `404 Not Found` 或 `400 Bad Request`  
  - 返回的错误消息应明确指示无效的用户ID

- **输入数据**：  
  - `productId`: 99999（无效的商品ID）

- **预期结果**：  
  - 状态码应为 `404 Not Found`  
  - 返回的错误消息应为 `Product not found` 或类似的说明

#### **4. 负载和并发测试**

**目标**：测试系统在高并发情况下的响应能力，确保系统在压力下稳定运行。

- **输入数据**：  
  - `userId`: 123  
  - `productId`: 456  
  - `quantity`: 2  
  - 模拟 1000 个并发请求

- **预期结果**：  
  - 每个请求应返回状态码 `200 OK`  
  - 每个订单返回正确的数据（订单号、商品名称、价格等）  
  - 响应时间应在可接受的范围内，通常在 2 秒以内  

- **工具**：Postman 结合 **Runner** 或 **JMeter** 用于模拟并发请求

#### **5. 安全性测试（身份验证和权限控制）**

**目标**：验证接口的身份验证和权限控制机制是否正常工作，确保只有合法用户才能下单。

- **输入数据**：  
  - 无效的 `Authorization` 头部（例如，JWT 令牌过期或无效）

- **预期结果**：  
  - 状态码应为 `401 Unauthorized` 或 `403 Forbidden`  
  - 返回的错误消息应指示用户需要进行身份验证或权限不足

#### **6. 订单取消接口的验证**

**目标**：验证订单创建后，用户是否能够正确取消订单，并且系统是否返回正确的响应。

- **输入数据**：  
  - `orderId`: 123456  

- **请求**：
  ```bash
  DELETE https://api.foodorder.com/orders/123456
  ```

- **预期结果**：  
  - 状态码应为 `200 OK`  
  - 返回的响应应确认订单取消  
  - 系统应不再显示该订单信息  

---

#### **测试脚本示例：**

```javascript
// 正常情况：验证状态码和返回值
pm.test("Status code is 200", function () {
    pm.response.to.have.status(200);
});

pm.test("Response contains orderId", function () {
    pm.response.to.have.jsonBody('orderId');
});

pm.test("Product is Test Dish", function () {
    pm.response.to.have.jsonBody('product').that.equals("Test Dish");
});

pm.test("Total price is correct", function () {
    pm.response.to.have.jsonBody('totalPrice').that.equals(20.0);
});
```

---

#### **总结：**

通过这些复杂的接口测试用例，我不仅验证了 **订单提交接口** 的基本功能，还进行了边界测试、错误处理、并发压力测试和安全性验证。通过全面的测试，确保接口的可靠性、稳定性和正确性。
