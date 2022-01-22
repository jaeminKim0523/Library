

목차

HealthKit 프로젝트 설정

개발 코드

실시간 심박수 스크린샷

HealthKit 프로젝트 설정

프로젝트 설정

Target → Signing & Capabilities → + Capability → HealthKit 추가

info.plist

Key: Privacy - Health Share Usage Description Value: HealthKit 정보 공유를 위한 권한 설명 작성

Key: Privacy - Health Update Usage Description Value: HealthKit 정보를 업데이트를 위한 권한 설명 작성

개발 코드

HealthKit을 통해 데이터를 받아오기 위한 코드 정리

필요 목록

정보 타입

정보의 시작/종료 날짜와 시간

시작과 종료 시간에 해당하는 샘플

읽기/쓰기 권한 설정

쿼리 선언 및 실행

HealthKit Session, Builder 선언 및 실행

전체 코드

1. 정보 타입

// 얻고자 하는 정보의 타입을 정한다.
let type = HKObjectType.quantityType(forIdentifier: HKQuantityTypeIdentifier.heartRate)!


타입 정보 링크:

Apple Developer Documentation

2. 정보의 시작/종료 날짜와 시간

// 캘린더 선언
let calendar = Calendar(identifier: Calendar.Identifier.gregorian)
// 오늘 날짜의 시작 시간
let startDate = cal.startOfDay(for: Date())
// 현재 시각
let endDate = Date()


3. 시작과 종료 시간에 해당하는 샘플

// 시작 시간과 종료 시간 사이의 샘플 정보에 대한 샘플
let predicate = HKQuery.predicateForSamples(withStart: startDate, end: endDate, options: .strictStartDate)


4. 읽기/쓰기 권한 설정

// 쓰기 권한 배열
let writeTypes: Set<HKSampleType> = [.workoutType(),
                                     HKSampleType.quantityType(forIdentifier: .heartRate)!,
                                     HKSampleType.quantityType(forIdentifier: .activeEnergyBurned)!,
                                     HKSampleType.quantityType(forIdentifier: .stepCount)!,
                                     HKSampleType.quantityType(forIdentifier: .distanceCycling)!,
                                     HKSampleType.quantityType(forIdentifier: .distanceSwimming)!,
                                     HKSampleType.quantityType(forIdentifier: .distanceWalkingRunning)!,
                                     HKSampleType.quantityType(forIdentifier: .swimmingStrokeCount)!]

// 읽기 권한 배열
let readTypes: Set<HKObjectType> = [.activitySummaryType(), .workoutType(),
	                                  HKObjectType.quantityType(forIdentifier: .heartRate)!,
	                                  HKObjectType.quantityType(forIdentifier: .activeEnergyBurned)!,
	                                  HKObjectType.quantityType(forIdentifier: .stepCount)!,
	                                  HKObjectType.quantityType(forIdentifier: .distanceCycling)!,
	                                  HKObjectType.quantityType(forIdentifier: .distanceSwimming)!,
	                                  HKObjectType.quantityType(forIdentifier: .distanceWalkingRunning)!,
	                                  HKObjectType.quantityType(forIdentifier: .swimmingStrokeCount)!]

// 권한 검사
healthStore.requestAuthorization(toShare: writeTypes, read: readTypes) { isSuccess, error in
    
}


권한 오류 발생시: 워치 → 설정 → 건강 → 앱 → HealthKit을 사용중인 App → 권한 설정

5. 쿼리 선언 및 실행

let query = HKStatisticsQuery(quantityType: type, quantitySamplePredicate: predicate, options: .discreteAverage) { query, results, error in
    
    let quantity: HKQuantity? = results?.averageQuantity()
    let beats: Double? = quantity?.doubleValue(for: HKUnit.count().unitDivided(by: HKUnit.minute()))
    print("got: \\(String(format: "%.f", beats!))")
    
}

healthStore.execute(query)


6. HealthKit Session, Builder 선언 및 실행

session = try HKWorkoutSession(healthStore: healthStore, configuration: configuration)
builder = session?.associatedWorkoutBuilder()

session?.delegate = self
builder?.delegate = self

session?.startActivity(with: date)
builder?.beginCollection(withStart: date, completion: { isSuccess, error in
    guard isSuccess else { return }
    
    
})


7. 전체 코드

// 얻고자 하는 정보의 타입을 정한다.
let type = HKObjectType.quantityType(forIdentifier: HKQuantityTypeIdentifier.heartRate)!
// 캘린더 선언
let calendar = Calendar(identifier: Calendar.Identifier.gregorian)
// 오늘 날짜의 시작 시간
let startDate = cal.startOfDay(for: Date())
// 현재 시각
let endDate = Date()
// 시작 시간과 종료 시간 사이의 샘플 정보에 대한 샘플
let predicate = HKQuery.predicateForSamples(withStart: startDate, end: endDate, options: .strictStartDate)
// 쓰기 권한 배열
let writeTypes: Set<HKSampleType> = [.workoutType(),
                                     HKSampleType.quantityType(forIdentifier: .heartRate)!,
                                     HKSampleType.quantityType(forIdentifier: .activeEnergyBurned)!,
                                     HKSampleType.quantityType(forIdentifier: .stepCount)!,
                                     HKSampleType.quantityType(forIdentifier: .distanceCycling)!,
                                     HKSampleType.quantityType(forIdentifier: .distanceSwimming)!,
                                     HKSampleType.quantityType(forIdentifier: .distanceWalkingRunning)!,
                                     HKSampleType.quantityType(forIdentifier: .swimmingStrokeCount)!]
// 읽기 권한 배열
let readTypes: Set<HKObjectType> = [.activitySummaryType(), .workoutType(),
	                                  HKObjectType.quantityType(forIdentifier: .heartRate)!,
	                                  HKObjectType.quantityType(forIdentifier: .activeEnergyBurned)!,
	                                  HKObjectType.quantityType(forIdentifier: .stepCount)!,
	                                  HKObjectType.quantityType(forIdentifier: .distanceCycling)!,
	                                  HKObjectType.quantityType(forIdentifier: .distanceSwimming)!,
	                                  HKObjectType.quantityType(forIdentifier: .distanceWalkingRunning)!,
	                                  HKObjectType.quantityType(forIdentifier: .swimmingStrokeCount)!]

// 권한 검사
healthStore.requestAuthorization(toShare: writeTypes, read: readTypes) { isSuccess, error in
    
}

// 사용자 HealthKit 정보를 얻기 위한 쿼리 생성
let query = HKStatisticsQuery(quantityType: type, quantitySamplePredicate: predicate, options: .discreteAverage) { query, results, error in
    
    let quantity: HKQuantity? = results?.averageQuantity()
    let beats: Double? = quantity?.doubleValue(for: HKUnit.count().unitDivided(by: HKUnit.minute()))
    print("got: \\(String(format: "%.f", beats!))")
    
}
// 쿼리 실행
healthStore.execute(query)

// 세션, 빌더 생성
session = try HKWorkoutSession(healthStore: healthStore, configuration: configuration)
builder = session?.associatedWorkoutBuilder()

// Delegate Pattern
session?.delegate = self
builder?.delegate = self

// 세션 시작
session?.startActivity(with: date)
builder?.beginCollection(withStart: date, completion: { isSuccess, error in
    guard isSuccess else { return }
    
    
})


실시간 심박수 캡처 화면

 

