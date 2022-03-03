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
  func centralManager(_ central: CBCentralManager, didConnect peripheral: CBPeripheral) {
    connectedPeripheral = peripheral
    connectedPeripheral?.delegate = self

    if let idx = self.peripherals.firstIndex(where: { $0.identifier == peripheral.identifier }) {
        peripherals[idx] = peripheral
    }
  }
```



# CoreBluetooth - Disconnect
```Swift
  func centralManager(_ central: CBCentralManager, didDisconnectPeripheral peripheral: CBPeripheral, error: Error?) {
    print("did disconnect Peripheral Identifier: \(peripheral.identifier.uuidString)")
    
    connectedPeripheral = nil
  }
```



[CoreBluetooth]: https://github.com/jaeminKim0523/Library/blob/main/CoreBluetooth.md "Read CoreBluetooth"
