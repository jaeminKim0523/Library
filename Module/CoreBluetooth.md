# CoreBluetooth

### 개발 노트
- CoreBluetooth를 이용하여 블루투스 리스트를 받아오는 기능을 개발한다.
- 블루투스 스캔을 한다.
- 블루투스 스캔을 취소한다.
- 스캔을 통해 받아온 블루투스 데이터들을 배열에 넣는다.

```Swift
class BLEService: NSObject {

  var centralManager: CBCentralManager
  
  var peripherals: [CBPeripheral] = []
  
  init() {
    // 블루투스 리스트를 받아오기 위한 Manager 선언
    self.centralManger = CBCentralManager(delegate: self, queue: .main)
  }
  
  // CentralManager를 통해 주변 블루투스를 찾는다.
  // 블루투스를 새로 스캔하기 시작 할 때 현재 블루투스의 리스트를 지운다.
  func startScan() {
      self.peripherals.removeAll()
      self.centralManager.scanForPeripherals(withServices: nil, options: nil)
  }
  
  // CentralManager의 스캔을 중지시킨다.
  func stopScan() {
      self.centralManager.stopScan()
  }
}
```

***
### CentralManagerDelegate
```
extension BLEService: CBCentralManagerDelegate {
  // CentralManager의 상태가 바뀌면 불린다.
  func centralManagerDidUpdateState(_ central: CBCentralManager) {
    // central.state를 통해 현재 CentralManager의 state를 가져 올 수 있다.
    let state = central.state
  }
  
  // CentralManager가 블루투스를 스캔하여 찾을 때마다 불린다.
  func centralManager(_ central: CBCentralManager, didDiscover peripheral: CBPeripheral, advertisementData: [String : Any], rssi RSSI: NSNumber) {
    // 현재 발견된 블루투스가 리스트에 이미 있는 블루투스인지 검사를 하고 없으면 추가를 있으면 이미 있는 블루투스에 다시 넣는다.
    if let idx = peripherals.firstIndex(where: { $0.identifier == peripheral.identifier }) {
        peripherals[idx] = peripheral
    } else {
        peripherals.append(peripheral)
    }
  }
}
```
