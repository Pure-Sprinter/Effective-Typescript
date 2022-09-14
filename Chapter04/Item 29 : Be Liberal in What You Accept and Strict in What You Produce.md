## Item 29: Be Liberal in What You Accept and Strict in What You Produce

함수 내에서 입력이 광범위한 것은 괜찮지만 일반적으로 출력으로 생성하는 요소는 더 구체적이어야 한다. 예를 들어 3D 매핑 API가 카메라에서 바운딩 박스를 계산하는 것을보여준다면 아래와 같이 2가지가 있다.

```typescript
declare function setCamera(camera: CameraOptions): void;
declare function viewportForBounds(bounds: LngLatBounds): CameraOptions;

interface CameraOptions {
  center?: LngLat;
  zoom?: number;
  bearing?: number;
  pitch?: number;
}

type LngLat =
  | { lng: number; lat: number }
  | { lon: number; lat: number }
  | [number, number];
```

`CameraOptions`에 존재하는 필드들은 모두 선택적으로 당신이 줌을 선택할 수도 있고 중앙에 초점을 둘 수 있기 때문이다. 선택적 특성 및 결합 유형이 많은 반환 유형으로 인해 ViewPort ForBounds를 사용하기 어렵다. 넓은 파라미터 타입은 편리하지만 넓은 리턴 타입은 그렇지 않다. 더 편리한 API는 그것이 생산하는 것에 엄격할 것이다.

이를 위한 한 가지 방법은 좌표의 표준 형식을 구별하는 것이다. "Array"와 "Array-like"를 구분하는 자바스크립트의 규칙에 따라 LngLat과 LngLatLike를 구분할 수 있다. 또한 완전히 정의된 카메라 유형과 setCamera에서 허용하는 부분 버전을 구분할 수도 있다.

```typescript
interface LngLat {
  lng: number;
  lat: number;
}
type LngLatLike = LngLat | { lon: number; lat: number } | [number, number];
interface Camera {
  center: LngLat;
  zoom: number;
  bearing: number;
  pitch: number;
}

interface CameraOptions extends Omit<Partial<Camera>, "center"> {
  center?: LngLatLike;
}
type LngLatBounds =
  | { northeast: LngLatLike; southwest: LngLatLike }
  | [LngLatLike, LngLatLike]
  | [number, number, number, number];

declare function setCamera(camera: CameraOptions): void;
declare function viewportForBounds(bounds: LngLatBounds): Camera;
```

### 기억해야할 것!!

1. 입력 유형은 출력 유형보다 광범위한 경향이 있다. 선택적 특성 및 결합 유형은 반환 유형보다 매개 변수 유형에서 더 일반적이다.
2. 매개 변수와 반환 유형 간에 유형을 재사용하려면 표준 형식(반환 유형)과 느슨한 형식(매개 변수)을 도입해야 한다.
