# Flutter 공부 관련해서 도움되는 링크 

Clean Architecture 기반으로 잘 설명되어 있는 링크 

- https://www.youtube.com/watch?v=r_OlGCOmeqk&t=231s
- https://github.com/rddewan/YouCanCode-Mobile/blob/main/lib/common/exception/failure.freezed.dart


# Library

### Flutter

#### Table Class

- https://api.flutter.dev/flutter/widgets/Table-class.html

#### Row

- [Row 안의 내용을 정렬하는 몇가지 방법](https://justdoooit.tistory.com/3)

#### Dio

- [Dio](https://velog.io/@hwibinissuccess/Flutter%E3%85%A3Dio-%ED%86%B5%ED%95%A9-%EC%A0%95%EB%A6%AC)

##### Basic

```dart
var dio = Dio(); 

dio.options.baseUrl = 'https://www.xx.com/api';
dio.options.connectTimeout = 5000; //5s
dio.options.receiveTimeout = 3000;

var options = BaseOptions(
  baseUrl: 'https://www.xx.com/api',
  connectTimeout: 5000,
  receiveTimeout: 3000,
);

Dio dio = Dio(options);


// BaseOptions 객체
BaseOptions({
    String? method,
    int? connectTimeout,
    int? receiveTimeout,
    int? sendTimeout,
    String baseUrl = '',
    Map<String, dynamic>? queryParameters,
    Map<String, dynamic>? extra,
    Map<String, dynamic>? headers,
    ResponseType? responseType = ResponseType.json,
    String? contentType,
    ValidateStatus? validateStatus,
    bool? receiveDataWhenStatusError,
    bool? followRedirects,
    int? maxRedirects,
    RequestEncoder? requestEncoder,
    ResponseDecoder? responseDecoder,
    ListFormat? listFormat,
    this.setRequestContentTypeWhenNoPayload = false,
  })
```

##### Interceptor

```dart 
class Interceptor {
  void onRequest(
    RequestOptions options,
    RequestInterceptorHandler handler,
  ) =>
      handler.next(options);

  void onResponse(
    Response response,
    ResponseInterceptorHandler handler,
  ) =>
      handler.next(response);

  void onError(
    DioError err,
    ErrorInterceptorHandler handler,
  ) =>
      handler.next(err);
}
```

## Basic

- 프로젝트 생성하기
- 라이브러리 추가하기
    - pub.dev 사용하기
    - pubspec.yaml
        -  [http](https://pub.dev/packages/http/install)
- 파일명은 소문자로 스네이크 케이스로 작성이 기본 규칙

### Model

- ? 의 여부는 nullable로 처리하겠다는 약속
- [Json to Dart Null Safety - Json에 대해서 Null Safety를 만들어줌](https://www.webinovers.com/web-tools/json-to-dart-convertor)