# 서론
이번 글은 이전에 작성하였던 글에 이어서 작성할 것이기 때문에 현재 글을 읽기전에 [CoreBluetooth]를 보고 오는 것이 좋다.  
```Swift
protocol BLEServiceProtocol {
  var centralManager: CBCentralManager { get set }
  var connectedPeripheral: CBPeripheral? { get set }
  
  var peripherals: [CBPeripheral] { get set }
}
```
이전 글에서 작성한 현재 클래스의 기본 값들을 보기위해 이전 글로 이동하는 것은 불편하니 Protocol을 이용하여 간단하게 작성하였다.  

# CoreBluetooth - Connect
```Swift
  func connectPeripheral(id: String) {
    guard let filtered = self.peripherals.first(where: { $0.identifier.uuidString == id }) else { return }

    print("will Connect Peripheral Identifier: \(id)")
    self.centralManager?.connect(filtered, options: nil)
  }

  func centralManager(_ central: CBCentralManager, didConnect peripheral: CBPeripheral) {
    connectedPeripheral = peripheral
    connectedPeripheral?.delegate = self

    if let idx = self.peripherals.firstIndex(where: { $0.identifier == peripheral.identifier }) {
        peripherals[idx] = peripheral
    }
  }
```
블루투스가 연결에 성공하였을때 불리는 Delegate pattern 메소드이다.
연결이 완료된 블루투스(peripheral)을 connectedPeripheral에 넣어주고 peripheral의 delegate를 받는다.

연결된 peripheral이 현재 검색된 블루투스 리스트에 존재하면 교체한다.


# CoreBluetooth - Disconnect
```Swift
  func disconnectPeripheral(id: String) {
    guard let filtered = self.peripherals.first(where: { $0.identifier.uuidString == id }) else { return }

    self.centralManager?.cancelPeripheralConnection(filtered)
  }
  
  func centralManager(_ central: CBCentralManager, didDisconnectPeripheral peripheral: CBPeripheral, error: Error?) {
    print("did disconnect Peripheral Identifier: \(peripheral.identifier.uuidString)")
    
    connectedPeripheral = nil
  }
```
블루투스가 연결이 종료되면 불리는 Delegate pattern 메소드이다.
연결이 끊기면 connectedPeripheral를 null로 만들어준다.


[CoreBluetooth]: https://github.com/jaeminKim0523/Library/blob/main/CoreBluetooth.md "Read CoreBluetooth"
