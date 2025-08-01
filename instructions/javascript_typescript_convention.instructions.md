---
applyTo: '**/*.{js,ts,jsx,tsx}'
---
Cung cấp ngữ cảnh dự án và hướng dẫn viết mã mà AI nên tuân theo khi tạo code, trả lời câu hỏi, hoặc xem xét thay đổi.

# Quy tắc viết JavaScript/TypeScript cho dự án

1. **Ngôn ngữ giao tiếp**:
   - Tất cả các phản hồi, tài liệu, và hướng dẫn phải được viết bằng tiếng Việt.
   - Tên biến, hàm, class, và các thành phần mã nguồn khác phải sử dụng tiếng Anh.

2. **Quy tắc đặt tên**:
   - Tên biến/hàm: Sử dụng kiểu `camelCase` (ví dụ: `userName`, `calculateTotal`).
   - Tên class: Sử dụng kiểu `PascalCase` (ví dụ: `UserProfile`, `DataProcessor`).
   - Tên constants: Sử dụng kiểu `UPPER_CASE` (ví dụ: `MAX_RETRY_COUNT`, `API_BASE_URL`).
   - Tên file: Sử dụng kiểu `kebab-case` (ví dụ: `user-service.ts`, `data-processor.js`).
   - Tên interface (TypeScript): Sử dụng tiền tố `I` (ví dụ: `IUserData`, `IApiResponse`).
   - Tên type (TypeScript): Sử dụng kiểu `PascalCase` (ví dụ: `UserType`, `ResponseData`).

3. **TypeScript specific rules**:
   - Luôn sử dụng TypeScript cho new projects.
   - Khai báo types rõ ràng cho tất cả parameters và return values.
   - Sử dụng interfaces cho object shapes.
   - Sử dụng enums cho predefined values.
   
   ```typescript
   interface IUserData {
     id: number;
     name: string;
     email: string;
     createdAt: Date;
   }
   
   enum UserStatus {
     ACTIVE = 'active',
     INACTIVE = 'inactive',
     PENDING = 'pending'
   }
   
   function processUserData(userData: IUserData): Promise<boolean> {
     // Implementation
   }
   ```

4. **Cấu trúc file và import**:
   ```typescript
   /**
    * Mô tả module: Module này chứa các utility functions cho xử lý user data
    * Tác giả: [Tên tác giả]
    * Ngày tạo: [Ngày tạo]
    */
   
   // External libraries
   import axios from 'axios';
   import moment from 'moment';
   
   // Internal imports
   import { IUserData, UserStatus } from '../types/user.types';
   import { logger } from '../utils/logger';
   
   // Constants
   const MAX_RETRY_COUNT = 3;
   const API_TIMEOUT = 5000;
   ```

5. **Tài liệu và chú thích**:
   - Sử dụng JSDoc format cho documentation:
   ```typescript
   /**
    * Tính toán điểm tín dụng cho khách hàng
    * 
    * @param userData - Thông tin khách hàng
    * @param modelVersion - Phiên bản model sử dụng
    * @returns Promise trả về điểm tín dụng
    * 
    * @example
    * ```typescript
    * const userData = { id: 1, income: 50000, age: 35 };
    * const score = await calculateCreditScore(userData, 'v2.1');
    * console.log(`Điểm tín dụng: ${score}`);
    * ```
    */
   async function calculateCreditScore(
     userData: IUserData, 
     modelVersion: string = 'v1.0'
   ): Promise<number> {
     // Implementation
   }
   ```

6. **Error handling**:
   ```typescript
   import { logger } from '../utils/logger';
   
   async function fetchUserData(userId: number): Promise<IUserData | null> {
     try {
       logger.info(`Bắt đầu lấy dữ liệu user ID: ${userId}`);
       
       const response = await axios.get(`/api/users/${userId}`, {
         timeout: API_TIMEOUT
       });
       
       logger.info(`Lấy dữ liệu thành công cho user ID: ${userId}`);
       return response.data;
       
     } catch (error) {
       if (axios.isAxiosError(error)) {
         logger.error(`API Error: ${error.message}`, { userId, status: error.response?.status });
       } else {
         logger.error(`Unexpected error: ${error}`, { userId });
       }
       return null;
     }
   }
   ```

7. **Async/Await patterns**:
   ```typescript
   // Prefer async/await over Promises
   async function processMultipleUsers(userIds: number[]): Promise<IUserData[]> {
     const results: IUserData[] = [];
     
     // Process sequentially if order matters
     for (const id of userIds) {
       const userData = await fetchUserData(id);
       if (userData) {
         results.push(userData);
       }
     }
     
     // Process in parallel if order doesn't matter
     const promises = userIds.map(id => fetchUserData(id));
     const responses = await Promise.allSettled(promises);
     
     return responses
       .filter((result): result is PromiseFulfilledResult<IUserData> => 
         result.status === 'fulfilled' && result.value !== null
       )
       .map(result => result.value);
   }
   ```

8. **Testing conventions**:
   - Sử dụng Jest cho unit testing.
   - File naming: `[module-name].test.ts` hoặc `[module-name].spec.ts`.
   ```typescript
   describe('UserService', () => {
     describe('calculateCreditScore', () => {
       it('should return correct score for valid user data', async () => {
         // Test implementation với mô tả bằng tiếng Việt
         const userData: IUserData = {
           id: 1,
           income: 50000,
           age: 35
         };
         
         const score = await calculateCreditScore(userData);
         
         expect(score).toBeGreaterThan(0);
         expect(score).toBeLessThanOrEqual(1);
       });
     });
   });
   ```

9. **Code formatting và linting**:
   - Sử dụng Prettier cho code formatting.
   - Sử dụng ESLint với TypeScript rules.
   - Configuration files:
     ```json
     // .prettierrc
     {
       "semi": true,
       "trailingComma": "es5",
       "singleQuote": true,
       "printWidth": 80,
       "tabWidth": 2
     }
     ```

10. **Performance best practices**:
    - Sử dụng `const` thay vì `let` khi có thể.
    - Implement proper memoization cho expensive calculations.
    - Sử dụng proper data structures (Map, Set) khi appropriate.
    - Avoid unnecessary re-renders trong React components.

11. **Quy trình commit**:
    - Tin nhắn commit phải được viết bằng tiếng Việt.
    - Format: `[loại]: mô tả ngắn gọn`
    - Ví dụ: `feat: thêm API endpoint cho tính toán credit score`
    - Loại commit: `feat`, `fix`, `docs`, `style`, `refactor`, `test`, `chore`
