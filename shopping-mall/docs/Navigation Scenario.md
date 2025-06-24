# Navigation for Shopping Mall

## 1. 사용자 인증 (Authentication)

### 회원가입 시나리오

```navigation
HomePage --> SignupPage : 회원가입 버튼 클릭
SignupPage --> (/api/auth/signup) : 회원가입 정보 입력 후 제출
(/api/auth/signup) --> SignupPage : 회원가입 실패, 아이디 중복 또는 유효성 검사 실패
(/api/auth/signup) --> LoginPage : 회원가입 성공 후 로그인 페이지로 이동
```

### 로그인 시나리오

```navigation
HomePage --> LoginPage : 로그인 버튼 클릭
LoginPage --> (/api/auth/login) : 로그인 정보 입력 후 제출
(/api/auth/login) --> LoginPage : 로그인 실패, 아이디 또는 비밀번호 불일치
(/api/auth/login) --> HomePage : 로그인 성공
```

## 2. 상품 탐색 (Product Discovery)

### 상품 검색 시나리오

```navigation
HomePage --> (search_products) : 검색어 입력 후 검색 버튼 클릭
(search_products) --> (/api/products/search)
(/api/products/search) --> ProductListPage : 검색 결과 반환 성공
(/api/products/search) --> HomePage : 검색 결과 없음 또는 오류 발생
```

### 상품 상세 조회 시나리오

```navigation
HomePage --> ProductListPage : 카테고리 메뉴 클릭
ProductListPage --> ProductDetailPage : 특정 상품 클릭
ProductDetailPage --> (/api/products/detail) : 페이지 진입 시 상품 상세 정보 요청
(/api/products/detail) --> ProductDetailPage : 데이터 로딩 성공
(/api/products/detail) --> ErrorPage : 상품 정보를 찾을 수 없음
```

## 3. 장바구니 (Cart)

### 장바구니 추가 및 조회 시나리오

```navigation
ProductDetailPage --> (add_to_cart) : '장바구니에 담기' 버튼 클릭
(add_to_cart) --> (/api/cart/add)
(/api/cart/add) --> (toast_success_message) : 추가 성공
(toast_success_message) --> ProductDetailPage
(/api/cart/add) --> (toast_error_message) : 추가 실패, 재고 부족
(toast_error_message) --> ProductDetailPage

HomePage --> CartPage : 장바구니 아이콘 클릭
CartPage --> (/api/cart/items) : 페이지 진입 시 장바구니 목록 요청
(/api/cart/items) --> CartPage : 목록 로딩 성공
(/api/cart/items) --> CartPage : 장바구니가 비어있음
```

### 장바구니 수정 시나리오

```navigation
CartPage --> (update_quantity) : 상품 수량 변경
(update_quantity) --> (/api/cart/update)
(/api/cart/update) --> CartPage : 수량 변경 성공 후 페이지 새로고침

CartPage --> (remove_item) : 상품 삭제 버튼 클릭
(remove_item) --> (/api/cart/remove)
(/api/cart/remove) --> CartPage : 상품 삭제 성공 후 페이지 새로고침
```

## 4. 주문 및 결제 (Order & Payment)

### 주문 및 결제 시나리오

```navigation
CartPage --> CheckoutPage : '주문하기' 버튼 클릭
CheckoutPage --> (/api/user/address) : 배송지 정보 로딩
CheckoutPage --> (validate_form) : '결제하기' 버튼 클릭 시
(validate_form) --> (request_payment) : 폼 데이터 유효
(validate_form) --> CheckoutPage : 폼 데이터 유효하지 않음

(request_payment) --> (/api/orders/create) : 주문 생성 요청
(/api/orders/create) --> OrderCompletePage : 주문 및 결제 성공
(/api/orders/create) --> CheckoutPage : 주문 실패, 결제 실패 또는 재고 변경
```

## 5. 주문 내역 (Order History)

### 주문 완료 및 내역 조회 시나리오

```navigation
OrderCompletePage --> MyPage : '주문 내역 보기' 클릭
OrderCompletePage --> HomePage : '쇼핑 계속하기' 클릭

MyPage --> OrderHistoryPage : '주문 내역' 메뉴 클릭
OrderHistoryPage --> (/api/orders/history) : 페이지 진입 시 주문 목록 요청
(/api/orders/history) --> OrderHistoryPage : 목록 로딩 성공

OrderHistoryPage --> OrderDetailPage : 특정 주문 내역 클릭
OrderDetailPage --> (/api/orders/detail) : 페이지 진입 시 상세 내역 요청
(/api/orders/detail) --> OrderDetailPage : 상세 내역 로딩 성공
```