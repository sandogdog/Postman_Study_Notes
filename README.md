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

   在某些情况下，接口的性能也是必须验证的，尤其是当系统需要处理大量请求时。在 **鸿蒙平台** 项目中，我使用 Postman 配合其他工具进行性能测试，模拟大量并发请求，检查接口的响应时间和稳定性。

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

   在接口测试中，除了功能和性能外，安全性也是一个重要的测试点。在 **鸿蒙平台** 项目中，我使用 **Postman** 测试了接口的权限控制和身份验证功能。

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

### **总结：**

在项目中，我通过 **Postman** 进行接口测试，不仅验证了接口的功能是否正常，还通过设置自动化验证脚本提高了测试效率。此外，我还进行过接口的性能、负载和安全性测试，确保接口在高并发环境下稳定运行，并验证了权限控制和身份验证的正确性。

通过这种方式，Postman 帮助我全面、系统地验证了接口的各项功能，保证了系统的稳定性和数据的准确性。
