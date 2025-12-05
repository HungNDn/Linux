# Device Tree

## 1. Basic Device Tree Syntax (Cú pháp cơ bản DT)

### Khái niệm nhanh
Device Tree (DT) là cấu trúc dữ liệu mô tả phần cứng cho Linux Kernel, đặc biệt trong kiến trúc ARM/RISC-V.  
Tập tin thường có dạng `.dts` (source) và `.dtsi` (include).

### Cú pháp cơ bản
```dts
/ {
    model = "My Custom Board";
    compatible = "myvendor,myboard";

    soc {
        gic: interrupt-controller@1000 {
            compatible = "arm,gic-400";
            interrupt-controller;
            #interrupt-cells = <3>;
        };

        uart0: serial@4000 {
            compatible = "ns16550a";
            reg = <0x4000 0x100>;
            interrupt-parent = <&gic>; // (phandle) => UART0 lấy interrupt từ GIC.
            interrupts = <5>;
        };
    };
};
```
### Các thành phần chính

- Node(1 node ~ 1 device): 
```dts
    uart0: serial@4000 
```
- Properties: kiểu key-value:
```dts
    compatible = "ns16550a";
    reg = <0x4000 0x100>;
    interrupts = <5>;
```
- Phandles: tham chiếu tới node khác bằng &label.
```dts
    interrupt-parent = <&gic>;
```

## 2. Device Tree Inheritance (Kế thừa trong Device Tree)

### Ý nghĩa
DTS hỗ trợ “kế thừa” thông qua:

- Include: .dts lấy nội dung từ .dtsi
- Override: Node sau có thể ghi đè node trước
- Fragment merging bằng &{node} { ... }

### Ví dụ
```dts
base-soc.dtsi

soc {
    timer: timer@3000 {
        compatible = "riscv,timer";
        reg = <0x3000 0x100>;
    };
};
```

```dts
board.dts

#include "base-soc.dtsi"

/ {
    model = "MyBoard v1";
    soc {
        timer {
            interrupts = <3>;
        };
    };
};

```
### Giải thích:
File board.dts kế thừa và ghi đè thuộc tính interrupts của timer.


## 3. Device Tree Specifications & Bindings (Chuẩn & ràng buộc DT)
### Device Tree Specification
- (DT Spec) là tài liệu chuẩn hóa định nghĩa cấu trúc, cú pháp và cách thể hiện của Device Tree.

- Ví dụ binding của UART (Documentation/devicetree/bindings/serial/…yaml):

```yaml
properties:
  compatible:
    enum:
      - ns16550a

  reg:
    maxItems: 1

  interrupts:
    minItems: 1
    maxItems: 1
```

## 4. Building & Validating Device Trees
### Build

DTS → DTB được build bằng device-tree-compiler (dtc):
```bash
dtc -I dts -O dtb -o myboard.dtb myboard.dts
```
DTC cũng có thể chuyển đổi ngược DTB thành DTS (human-readable):
```bash
dtc -I dtb -O dts -o dump.dts myboard.dtb
```
Build tất cả DTBs cho kiến trúc
```bash
make ARCH=arm64 dtbs
```

### Validate (kiểm tra đúng schema)

Linux hỗ trợ kiểm tra DTS với YAML bindings:
```bash
make dt_binding_check
make dtbs_check
```
Nếu file DTS sai cấu trúc hoặc thiếu compatible, sẽ báo lỗi.


## 5. Một vài thuộc tính phổ biến trong Device Tree (Common properties)

| Thuộc tính        | Ý nghĩa chính                                    | Ví dụ & chú ý                             |
|-------------------|-------------------------------------------------|------------------------------------------|
| **compatible**    | Định danh kiểu thiết bị (để kernel biết driver) | `"ns16550a"` cho UART, `"riscv,timer"` cho timer |
| **reg**           | Địa chỉ vùng nhớ hoặc vùng IO thiết bị (base address + size) | `<0x4000 0x100>` là vùng địa chỉ bắt đầu 0x4000, dài 0x100 bytes |
| **interrupts**    | Mô tả các interrupt mà thiết bị sử dụng (số IRQ, type...) | `<5>` hoặc `<3 1 0>` tùy controller     |
| **interrupt-parent** | Node controller cung cấp interrupt (phandle)  | `<&gic>` tham chiếu đến interrupt-controller |
| **clocks**        | Liên kết với clock providers (phandle + id)     | `<&clk0 0>` chỉ clock đầu tiên từ `clk0` |
| **resets**        | Liên kết với reset controller                    | `<&rst 1>` reset thứ 1                    |
| **status**        | Trạng thái thiết bị, ví dụ `"okay"` hoặc `"disabled"` | `"okay"` có nghĩa thiết bị được enable  |
| **pinctrl-0**     | Liên kết cấu hình pin (phandle đến pin controller) | `<&pinctrl0>`                            |
| **reg-names**     | Đặt tên cho các vùng trong `reg`                 | `"control"`, `"status"`                   |
| **ranges**        | Ánh xạ địa chỉ vùng con sang vùng cha (bus mapping) | `ranges` thường dùng trong bus hoặc bridge |
| **dma-coherent**  | Thiết bị hỗ trợ DMA đồng bộ bộ nhớ                | property không giá trị, chỉ hiện diện là true |
| **clock-frequency** | Tần số clock của thiết bị                         | `clock-frequency = <24000000>;`          |
| **address-cell** | Số cell (32-bit words) dùng để biểu diễn địa chỉ của node con trong node cha. |`#address-cell = <1>;`          |
| **size-cell** | Số cell dùng để biểu diễn kích thước vùng nhớ của node con. | `#size-cell = <1>;`          |
