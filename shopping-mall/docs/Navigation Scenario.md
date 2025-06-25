# 쇼핑몰 내비게이션 시나리오

## 1. 사용자 인증 (Authentication)

### 회원가입 시나리오

```navigation
Home --> Terms : 회원가입 버튼 클릭
Terms --> Signup : 약관 동의 후 '다음' 클릭
Signup --> (/api/auth/signup) : 회원가입 정보 입력 후 제출
(/api/auth/signup) --> Signup : 회원가입 실패, 아이디 중복 또는 유효성 검사 실패
(/api/auth/signup) --> Home : 회원가입 및 자동 로그인 성공
```

### 로그인 시나리오

```navigation
Home --> Login : 로그인 버튼 클릭
Login --> (/api/auth/login) : 로그인 정보 입력 후 제출
(/api/auth/login) --> Login : 로그인 실패, 아이디 또는 비밀번호 불일치
(/api/auth/login) --> Home : 로그인 성공
```

## 2. 상품 탐색 (Product Discovery)

### 상품 검색 시나리오

```navigation
Home --> (search_products) : 검색어 입력 후 검색 버튼 클릭
(search_products) --> (/api/products/search)
(/api/products/search) --> ProductList : 검색 결과 반환 성공
(/api/products/search) --> Home : 검색 결과 없음 또는 오류 발생
```

### 상품 상세 조회 시나리오

```navigation
Home --> ProductList : 카테고리 메뉴 클릭
ProductList --> ProductDetail : 특정 상품 클릭
ProductDetail --> (/api/products/detail) : 페이지 진입 시 상품 상세 정보 요청
(/api/products/detail) --> ProductDetail : 데이터 로딩 성공
(/api/products/detail) --> Error : 상품 정보를 찾을 수 없음
```

## 3. 장바구니 (Cart)

### 장바구니 추가 및 조회 시나리오

```navigation
ProductDetail --> (add_to_cart) : '장바구니에 담기' 버튼 클릭
(add_to_cart) --> (/api/cart/add)
(/api/cart/add) --> (toast_success_message) : 추가 성공
(toast_success_message) --> ProductDetail
(/api/cart/add) --> (toast_error_message) : 추가 실패, 재고 부족
(toast_error_message) --> ProductDetail

Home --> Cart : 장바구니 아이콘 클릭
Cart --> (/api/cart/items) : 페이지 진입 시 장바구니 목록 요청
(/api/cart/items) --> Cart : 목록 로딩 성공
(/api/cart/items) --> Cart : 장바구니가 비어있음
```

### 장바구니 수정 시나리오

```navigation
Cart --> (update_quantity) : 상품 수량 변경
(update_quantity) --> (/api/cart/update)
(/api/cart/update) --> Cart : 수량 변경 성공 후 페이지 새로고침

Cart --> (remove_item) : 상품 삭제 버튼 클릭
(remove_item) --> (/api/cart/remove)
(/api/cart/remove) --> Cart : 상품 삭제 성공 후 페이지 새로고침
```

## 4. 주문 및 결제 (Order & Payment)

### 주문 및 결제 시나리오

```navigation
Cart --> Checkout : '주문하기' 버튼 클릭
Checkout --> (/api/user/address) : 배송지 정보 로딩
Checkout --> (validate_form) : '결제하기' 버튼 클릭 시
(validate_form) --> (request_payment) : 폼 데이터 유효
(validate_form) --> Checkout : 폼 데이터 유효하지 않음

(request_payment) --> (/api/orders/create) : 주문 생성 요청
(/api/orders/create) --> OrderComplete : 주문 및 결제 성공
(/api/orders/create) --> Checkout : 주문 실패, 결제 실패 또는 재고 변경
```

## 5. 주문 내역 (Order History)

### 주문 완료 및 내역 조회 시나리오

```navigation
OrderComplete --> My : '주문 내역 보기' 클릭
OrderComplete --> Home : '쇼핑 계속하기' 클릭

My --> OrderHistory : '주문 내역' 메뉴 클릭
OrderHistory --> (/api/orders/history) : 페이지 진입 시 주문 목록 요청
(/api/orders/history) --> OrderHistory : 목록 로딩 성공

OrderHistory --> OrderDetail : 특정 주문 내역 클릭
OrderDetail --> (/api/orders/detail) : 페이지 진입 시 상세 내역 요청
(/api/orders/detail) --> OrderDetail : 상세 내역 로딩 성공
```