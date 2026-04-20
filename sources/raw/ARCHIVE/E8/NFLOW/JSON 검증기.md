# 목적

- 시뮬레이션 설정 파일의 JSON 구조를 검증하여 잘못된 설정으로 인한 런타임 오류 방지
- 3D솔루션개발팀에 제공하여 JSON 생성부 개발에 참고자료로 활용

# 기술 스택

- **C++**: 메인 개발 언어
- **nlohmann/json**: JSON 파싱 라이브러리 (hearder-only)
- **valijson**: JSON Schema 검증 라이브러리 (hearder-only)
- **JSON Schema Draft 7**: 검증 규칙

# 아키텍쳐

## 파일 구조

```jsx
LBM_Runtime(or SPH3D_Runtime..)/
├── main.cpp                     # validation 호출
LBM(or SPH3D..)/
├── filemanager/
│   ├── json_validator.cpp       # JSON 검증
│   └── json_validator.hpp       # 검증 헤더
└── 3rdparty/
		├── nlohmann_json            # 외부 라이브러리 (파싱)
    └── valijson/                # 외부 라이브러리 (검증)
```

## 실행 흐름

```jsx
//main.cpp
//해석에 사용할 json과 스키마 json을 입력 
//스키마 json을 입력안하는 경우, 검증없이 해석 (현재는 검증 규칙이 DEN 케이스에만 한정되어 있음)
JsonValidator validator;
if(argc == 3){
  validator.Validate(convert_to_utf8(argv[1]), convert_to_utf8(argv[2]));
}
```

```jsx
//json_validator.cpp
void JsonValidator::Validate(const std::filesystem::path& path, const std::filesystem::path& schema_path){
  
  using namespace valijson;

  nlohmann::json input_json;
  nlohmann::json schema_json;

  // 입력 JSON 파일 로드 및 파싱
  try
  {
    std::ifstream f(path);
    input_json = nlohmann::json::parse(f);
  }
  catch (std::exception& e)
  {
    error(R"("Failed to parse input json file: )", e.what());
    std::exit(0);
  }

  // 스키마 JSON 파일 로드 및 파싱
  try
  {
    std::ifstream fs(schema_path);
    schema_json = nlohmann::json::parse(fs);
  }
  catch (std::exception& e)
  {
    error(R"("Failed to parse schema json file: )", e.what());
    std::exit(0);
  }

  // valijson 스키마 객체 생성 및 컴파일 
  Schema schema;
  SchemaParser parser;
  adapters::NlohmannJsonAdapter schemaAdapter(schema_json);
  parser.populateSchema(schemaAdapter, schema);

  // 검증 수행
  Validator validator;
  adapters::NlohmannJsonAdapter target(input_json);
  ValidationResults results;

  if (!validator.validate(schema, target, &results))
  {
    // 검증 실패 시 상세 오류 정보 출력
    ValidationResults::Error val_error;
    error(R"("JSON validation failed)");
    while (results.popError(val_error))
    {
      std::cerr << "  - Context: ";
      for (const auto& part : val_error.context) {
        std::cerr << part;
      }
      std::cerr << "\\n";
      
      std::cerr << "  - Description: ";
      for (const auto& part : val_error.description) {
        std::cerr << part;
      }
      std::cerr << "\\n\\n";
    }
    std::exit(0);
  }
  else
  {
    verbose("JSON validation passed successfully");
  }
}
```

# JSON Schema

## JSON Schema란

- JSON Schema는 JSON 데이터의 구조, 타입, 제약 조건을 설명하는 메타데이터
- 마치 데이터베이스의 테이블 스키마처럼 JSON의 "설계도" 역할

## 주요 목적

- **데이터 검증**: JSON이 올바른 구조인지 자동 확인
- **문서화**: 데이터 구조를 명확히 정의
- **개발 가이드**: API나 설정 파일의 형식 안내
- **오류 방지**: 잘못된 데이터로 인한 런타임 오류 예방

## 기본 구조

```jsx
{
  "$schema": "<http://json-schema.org/draft-07/schema#>",
  "title": "스키마 제목",
  "description": "스키마 설명",
  "type": "object",
  "properties": { ... },
  "required": [ ... ],
  "definitions": { ... }
}
```

- schema
    - 사용할 JSON Schema 버전을 명시
    - 검증 도구가 올바른 규칙을 적용하도록 안내
- title
    - 스키마 제목
- description
    - 스키마의 상세한 설명 제공
    - 개발자에게 스키마의 용도와 목적 설명
    - 문서화 및 도움말 표시용
- type
    - 스키마가 검증할 데이터의 기본 타입 지정
    - JSON의 7가지 기본 타입 중 하나 선택
- properties
    - 객체의 각 속성(필드)에 대한 스키마 정의
    - 객체 타입일 때 필수적으로 사용
    - 각 속성별로 타입, 제약조건, 설명 등 정의
- required
    - 반드시 존재해야 하는 필드들을 명시
    - 배열 형태로 필드명들을 나열
    - properties에 정의된 필드명과 일치해야 함
- definitions
    - 사용할 JSON Schema 버전을 명시
    - 검증 도구가 올바른 규칙을 적용하도록 안내


[250917_DELBM_SC_sedi_sample_schema.json](attachment:d3ef7258-cc61-477a-b123-252392e6f2e0:250917_DELBM_SC_sedi_sample_schema.json)