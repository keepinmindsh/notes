
```yaml
dependencies:
  flutter:
    sdk: flutter
  # https://pub.dev/packages/logging
  logging: ^1.3.0
```


```dart
import 'package:flutter/material.dart';
import 'package:flutter_test/flutter_test.dart';
import 'package:logging/logging.dart';

void main() {
  testWidgets('Counter increments smoke test', (WidgetTester tester) async {});

  test("Basic Test Code", () async {
    // mvvm 구조에서 순수 Business Logic 만 구현할 수 있도록 처리하기
  });
}

void listTest() {
  var list = [];
  list.add(1);
  list.add(2);
  list.add(3);
  list.add(4);
  list.add(5);
  list.clear();
}

void listMapTest() {
  List<Map<String, dynamic>> category = [
    {'id' : '0', 'name' : '수입' },
    {'id' : '1', 'name' : '수입1' },
    {'id' : '2', 'name' : '수입2' },
    {'id' : '3', 'name' : '수입3' },
  ];
  
  prints(category);
}

void listMapWithIconTest() {
  List<Map<String, dynamic>> categoryList = [
    {'id' : '0', 'name' : '수입' , 'icon' : const Icon(Icons.work_outline_rounded) },
    {'id' : '1', 'name' : '수입1', 'icon' : const Icon(Icons.work_outline_rounded) },
    {'id' : '2', 'name' : '수입2', 'icon' : const Icon(Icons.work_outline_rounded) },
    {'id' : '3', 'name' : '수입3', 'icon' : const Icon(Icons.work_outline_rounded) },
  ];

  var value = categoryList.firstWhere((element) => element['id'] == '0')['name'];
  

  Logger.root.level = Level.ALL; // defaults to Level.INFO
  Logger.root.onRecord.listen((record) {
    print(value);
  });
}

```

# MVVM 구조로 가져갔을 때 얻을 수 있는 이점 

- 순수 비즈니스 로직에 대해서 Test Code 기반으로 테스트가 가능해진다. 

```dart
import 'package:flutter_test/flutter_test.dart';
import 'package:lines_github_project_flutter/features/login/data/dto/request/login_request.dart';

void main() {
  test("Login Request Test", () async {
    final loginRequest = LoginRequest(
      email: "hongildong@gmail.com",
      password: "1q2w3e4r",
    );

    var json = loginRequest.toJson();

    expect(
        json,
        emitsInOrder([
          isA<Map<String, dynamic>>()
        ])
    );
  });
}
```

> https://www.inflearn.com/courses/lecture?courseId=328068&unitId=98560&tab=curriculum&subtitleLanguage=ko