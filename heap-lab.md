# ใบงานการทดลอง: โครงสร้างข้อมูล Heap

## วัตถุประสงค์การเรียนรู้
1. อธิบายหลักการทำงานของโครงสร้างข้อมูลแบบ Heap ได้
2. เขียนโปรแกรมเพื่อสร้างและจัดการ Heap ได้
3. ประยุกต์ใช้ Heap ในการแก้ปัญหาได้

## ทฤษฎีที่เกี่ยวข้อง
Heap เป็นโครงสร้างข้อมูลแบบต้นไม้แบบทวิภาค (Binary Tree) ที่มีคุณสมบัติพิเศษดังนี้:
- เป็นต้นไม้ที่สมบูรณ์ (Complete Binary Tree)
- ทุกโหนดพ่อแม่ต้องมีค่ามากกว่าหรือเท่ากับโหนดลูกทั้งสอง (กรณี Max Heap)
- หรือทุกโหนดพ่อแม่ต้องมีค่าน้อยกว่าหรือเท่ากับโหนดลูกทั้งสอง (กรณี Min Heap)

## ตัวอย่างการเขียนโปรแกรม

### การสร้าง Max Heap
```python
class MaxHeap:
    def __init__(self):
        self.heap = []
        
    def parent(self, i):
        return (i - 1) // 2
        
    def left_child(self, i):
        return 2 * i + 1
        
    def right_child(self, i):
        return 2 * i + 2
        
    def swap(self, i, j):
        self.heap[i], self.heap[j] = self.heap[j], self.heap[i]
        
    def insert(self, key):
        self.heap.append(key)
        self._sift_up(len(self.heap) - 1)
        
    def _sift_up(self, i):
        parent = self.parent(i)
        if i > 0 and self.heap[i] > self.heap[parent]:
            self.swap(i, parent)
            self._sift_up(parent)
```

### การลบค่าสูงสุด
```python
def extract_max(self):
    if len(self.heap) == 0:
        return None
    
    max_val = self.heap[0]
    self.heap[0] = self.heap[-1]
    self.heap.pop()
    
    if len(self.heap) > 0:
        self._sift_down(0)
    
    return max_val

def _sift_down(self, i):
    max_index = i
    left = self.left_child(i)
    right = self.right_child(i)
    
    if left < len(self.heap) and self.heap[left] > self.heap[max_index]:
        max_index = left
    
    if right < len(self.heap) and self.heap[right] > self.heap[max_index]:
        max_index = right
        
    if i != max_index:
        self.swap(i, max_index)
        self._sift_down(max_index)
```

## แบบฝึกหัด

### แบบฝึกหัดที่ 1: การแทรกค่า
จงเขียนโปรแกรมเพื่อแทรกค่าต่อไปนี้ลงใน Max Heap ที่ว่างเปล่า: 5, 3, 8, 1, 2, 7, 6, 4
พร้อมแสดงค่า Max Heap ที่ได้หลังจากเพิ่มข้อมูลครบแล้ว
```python
import heapq

class MaxHeap:
    def __init__(self):
        self.heap = []
    
    def insert(self, val):
        heapq.heappush(self.heap, -val)  # ใช้ค่าเชิงลบเพื่อจำลอง Max Heap
    
    def display(self):
        print("Max Heap:", [-val for val in self.heap])  # แปลงค่ากลับเป็นบวก

# สร้าง Max Heap
max_heap = MaxHeap()
values = [5, 3, 8, 1, 2, 7, 6, 4]

# แทรกค่าลงใน Max Heap
for val in values:
    max_heap.insert(val)

# แสดงผลลัพธ์ของ Max Heap
max_heap.display()
```
![image](https://github.com/user-attachments/assets/5d17f644-f2c7-49e1-b9b7-41d1b412d654)

### แบบฝึกหัดที่ 2: การลบค่า
จากข้อ 1 จงเขียนลำดับการลบค่าสูงสุดออกจาก Heap จำนวน 3 ครั้ง แสดงข้อมูล Heap หลังจากลบแต่ละครั้ง

```python
import heapq

class MaxHeap:
    def __init__(self):
        self.heap = []
    
    def insert(self, val):
        heapq.heappush(self.heap, -val)  # ใช้ค่าเชิงลบเพื่อจำลอง Max Heap
    
    def delete_max(self):
        if self.heap:
            return -heapq.heappop(self.heap)  # ลบและคืนค่ามากที่สุด
        return None  # ถ้า Heap ว่าง
    
    def display(self):
        print("Max Heap:", [-val for val in self.heap])  # แปลงค่ากลับเป็นบวก

# สร้าง Max Heap
max_heap = MaxHeap()
values = [5, 3, 8, 1, 2, 7, 6, 4]

# แทรกค่าลงใน Max Heap
for val in values:
    max_heap.insert(val)

# แสดงค่า Heap หลังจากแทรกข้อมูลครบ
print("Heap ก่อนลบค่า:")
max_heap.display()

# ลบค่ามากที่สุด 3 ครั้ง และแสดงผลลัพธ์แต่ละครั้ง
for i in range(3):
    max_value = max_heap.delete_max()
    print(f"\nลบค่ามากที่สุด: {max_value}")
    max_heap.display()
```
![image](https://github.com/user-attachments/assets/3998105b-3c4a-4243-be4f-901bf171b30a)

### แบบฝึกหัดที่ 3: การเขียนโปรแกรม
จงเขียนฟังก์ชัน `is_max_heap(arr)` ที่รับ array เข้ามาและตรวจสอบว่าป็น Max Heap หรือไม่ 

```python
def is_max_heap(arr):
    n = len(arr)
    
    # ตรวจสอบทุก parent (i) ว่ามีค่ามากกว่าหรือเท่ากับลูกทั้งสอง
    for i in range(n // 2):  # ตรวจเฉพาะ parent node เท่านั้น
        left = 2 * i + 1  # ดัชนีลูกซ้าย
        right = 2 * i + 2  # ดัชนีลูกขวา

        if left < n and arr[i] < arr[left]:  # ตรวจสอบลูกซ้าย
            return False
        if right < n and arr[i] < arr[right]:  # ตรวจสอบลูกขวา
            return False

    return True

# ทดสอบฟังก์ชัน
arr1 = [10, 5, 8, 3, 4, 7, 6]  # เป็น Max Heap
arr2 = [10, 20, 8, 3, 4, 7, 6]  # ไม่เป็น Max Heap (20 > 10)

print("True = เป็น Max-Heap\nFalse = ไม่เป็น Max-Heap\n")

print(f"arr1 = {arr1}, Is it a max-heap? = {is_max_heap(arr1)}")
print(f"arr2 = {arr2}, Is it a max-heap? = {is_max_heap(arr2)}")



```
![image](https://github.com/user-attachments/assets/c02e0893-0dc2-4afb-9acd-3b6cd2f0cd0f)

## การประยุกต์ใช้งานจริง

### 1. ระบบจัดการคิวผู้ป่วยฉุกเฉิน (Emergency Room Triage)
```python
class Patient:
    def __init__(self, name, priority):
        self.name = name
        self.priority = priority  # ระดับความรุนแรง 1-5 (1 = ฉุกเฉินมาก)
    
    def __lt__(self, other):
        return self.priority < other.priority

class ERQueue:
    def __init__(self):
        self.queue = []  # Min Heap เพื่อให้ผู้ป่วยที่มี priority ต่ำ (อาการหนัก) ได้รับการรักษาก่อน
    
    def add_patient(self, patient):
        heapq.heappush(self.queue, patient)
    
    def treat_next_patient(self):
        if self.queue:
            return heapq.heappop(self.queue)
        return None

# ตัวอย่างการใช้งาน
er = ERQueue()
er.add_patient(Patient("คนไข้ A", 1))  # หัวใจวาย
er.add_patient(Patient("คนไข้ B", 3))  # ปวดท้อง
er.add_patient(Patient("คนไข้ C", 2))  # กระดูกหัก
```

![image](https://github.com/user-attachments/assets/d3e6c268-7f34-4bab-ade3-943c23248ca7)


### 2. ระบบแนะนำสินค้าขายดี (Top K Items)
```python
class ProductRanking:
    def __init__(self, k):
        self.k = k
        self.products = []  # Min Heap เก็บ k สินค้าขายดีที่สุด
    
    def update_sales(self, product_id, sales):
        if len(self.products) < self.k:
            heapq.heappush(self.products, (sales, product_id))
        else:
            if sales > self.products[0][0]:
                heapq.heapreplace(self.products, (sales, product_id))
    
    def get_top_k(self):
        return sorted(self.products, reverse=True)

# ตัวอย่างการใช้งาน
ranking = ProductRanking(3)  # เก็บ 3 อันดับสินค้าขายดี
ranking.update_sales("สินค้า A", 100)
ranking.update_sales("สินค้า B", 150)
ranking.update_sales("สินค้า C", 80)
ranking.update_sales("สินค้า D", 200)
```
![image](https://github.com/user-attachments/assets/969baef7-884e-46d8-80ca-2270f989cd43)

### 3. ตัวอย่างระบบคิวธนาคาร แบบมีระดับความสำคัญ โดยใช้ heapq
```python 
import heapq
from datetime import datetime
import time

class BankCustomer:
    def __init__(self, name, service_type, is_premium=False):
        self.name = name
        self.service_type = service_type
        self.is_premium = is_premium
        self.arrival_time = datetime.now()
        
        # กำหนดลำดับความสำคัญตาม service_type และสถานะลูกค้า
        self.priority = self._calculate_priority()
        
    def _calculate_priority(self):
        # ลำดับความสำคัญ (ยิ่งน้อยยิ่งสำคัญ)
        priority = {
            'ฝาก-ถอน': 3,
            'ชำระค่าบริการ': 2,
            'เปิดบัญชี': 1,
            'สินเชื่อ': 0
        }
        
        # ลูกค้า Premium จะได้ priority สูงกว่าปกติ 1 ระดับ
        base_priority = priority.get(self.service_type, 4)
        if self.is_premium:
            base_priority -= 0.5
            
        return base_priority
        
    def __lt__(self, other):
        # เปรียบเทียบลำดับความสำคัญ
        if self.priority == other.priority:
            # ถ้าความสำคัญเท่ากัน ใช้เวลามาก่อน-หลัง
            return self.arrival_time < other.arrival_time
        return self.priority < other.priority
        
class BankQueue:
    def __init__(self):
        self.queue = []  # heap queue
        self.waiting_count = 0
        
    def add_customer(self, customer):
        heapq.heappush(self.queue, customer)
        self.waiting_count += 1
        print(f"ลูกค้า: {customer.name}")
        print(f"บริการ: {customer.service_type}")
        print(f"สถานะ: {'Premium' if customer.is_premium else 'ทั่วไป'}")
        print(f"จำนวนคิวรอ: {self.waiting_count}")
        print("-" * 30)
        
    def serve_next_customer(self):
        if not self.queue:
            print("ไม่มีลูกค้าในคิว")
            return None
            
        customer = heapq.heappop(self.queue)
        self.waiting_count -= 1
        
        wait_time = datetime.now() - customer.arrival_time
        print(f"\nเรียกลูกค้า: {customer.name}")
        print(f"บริการ: {customer.service_type}")
        print(f"เวลารอ: {wait_time.seconds} วินาที")
        print(f"จำนวนคิวรอ: {self.waiting_count}")
        print("-" * 30)
        
        return customer
        
    def display_queue(self):
        if not self.queue:
            print("ไม่มีลูกค้าในคิว")
            return
            
        print("\nรายการคิวที่รอ:")
        # สร้าง copy ของคิวเพื่อไม่ให้กระทบคิวจริง
        temp_queue = self.queue.copy()
        position = 1
        
        while temp_queue:
            customer = heapq.heappop(temp_queue)
            print(f"{position}. {customer.name} - {customer.service_type}")
            position += 1
        print("-" * 30)

# ตัวอย่างการใช้งาน
def demo_bank_queue():
    bank = BankQueue()
    
    # เพิ่มลูกค้าเข้าคิว
    customers = [
        BankCustomer("คุณแดง", "ฝาก-ถอน"),
        BankCustomer("คุณขาว", "สินเชื่อ", is_premium=True),
        BankCustomer("คุณเขียว", "ชำระค่าบริการ"),
        BankCustomer("คุณม่วง", "เปิดบัญชี"),
        BankCustomer("คุณฟ้า", "สินเชื่อ")
    ]
    
    for customer in customers:
        bank.add_customer(customer)
        time.sleep(1)  # จำลองการมาถึงต่างเวลากัน
        
    print("\nแสดงลำดับคิว:")
    bank.display_queue()
    
    # จำลองการเรียกลูกค้าเข้ารับบริการ
    print("\nเริ่มเรียกลูกค้า:")
    for _ in range(len(customers)):
        bank.serve_next_customer()
        time.sleep(1)

if __name__ == "__main__":
    demo_bank_queue()
```
### ทำการปรับปรุงโปรแกรม โดยให้ทำการกดเลือกธุรกรรม แล้วจะมีเลขคิวในแต่ละช่วงของการทำธุรกรรม แทนชื่อของลูกค้า และมีการเรียกคิวตามระดับความสำคัญและเวลาที่มาก่อนหลังของลูกค้า

```python
import heapq
from datetime import datetime
import time

class BankCustomer:
    def __init__(self, name, service_type, is_premium=False):
        self.name = name
        self.service_type = service_type
        self.is_premium = is_premium
        self.arrival_time = datetime.now()
        
        # กำหนดลำดับความสำคัญตาม service_type และสถานะลูกค้า
        self.priority = self._calculate_priority()
        
    def _calculate_priority(self):
        # ลำดับความสำคัญ (ยิ่งน้อยยิ่งสำคัญ)
        priority = {
            'ฝาก-ถอน': 3,
            'ชำระค่าบริการ': 2,
            'เปิดบัญชี': 1,
            'สินเชื่อ': 0
        }
        
        # ลูกค้า Premium จะได้ priority สูงกว่าปกติ 1 ระดับ
        base_priority = priority.get(self.service_type, 4)
        if self.is_premium:
            base_priority -= 1  # ให้ Premium มี priority ที่น้อยกว่า 1 ระดับ
            
        return base_priority
        
    def __lt__(self, other):
        # เปรียบเทียบลำดับความสำคัญ
        if self.priority == other.priority:
            # ถ้าความสำคัญเท่ากัน ใช้เวลามาก่อน-หลัง
            return self.arrival_time < other.arrival_time
        return self.priority < other.priority

        
class BankQueue:
    def __init__(self):
        self.queue = []  # heap queue
        self.waiting_count = 0
        
    def add_customer(self, customer):
        heapq.heappush(self.queue, customer)
        self.waiting_count += 1
        print(f"หมายเลขคิว: {self.waiting_count}")
        print(f"บริการ: {customer.service_type}")
        print(f"สถานะ: {'Premium' if customer.is_premium else 'ทั่วไป'}")
        print("-" * 30)
        
    def serve_next_customer(self):
        if not self.queue:
            print("ไม่มีลูกค้าในคิว")
            return None
            
        customer = heapq.heappop(self.queue)
        self.waiting_count -= 1
        
        wait_time = datetime.now() - customer.arrival_time
        print(f"\nเรียกลูกค้าหมายเลขคิว: {self.waiting_count + 1}")
        print(f"บริการ: {customer.service_type}")
        print(f"เวลารอ: {wait_time.seconds} วินาที")
        print(f"จำนวนคิวรอ: {self.waiting_count}")
        print("-" * 30)
        
        return customer
        
    def display_queue(self):
        if not self.queue:
            print("ไม่มีลูกค้าในคิว")
            return
            
        print("\nรายการคิวที่รอ:")
        # สร้าง copy ของคิวเพื่อไม่ให้กระทบคิวจริง
        temp_queue = self.queue.copy()
        position = 1
        
        while temp_queue:
            customer = heapq.heappop(temp_queue)
            print(f"{position}. บริการ: {customer.service_type} | คิว: {position}")
            position += 1
        print("-" * 30)

# ตัวอย่างการใช้งาน
def demo_bank_queue():
    bank = BankQueue()
    
    # เพิ่มลูกค้าเข้าคิว
    customers = [
        BankCustomer("คุณแดง", "ฝาก-ถอน"),
        BankCustomer("คุณขาว", "สินเชื่อ", is_premium=True),
        BankCustomer("คุณเขียว", "ชำระค่าบริการ"),
        BankCustomer("คุณม่วง", "เปิดบัญชี"),
        BankCustomer("คุณฟ้า", "สินเชื่อ")
    ]
    
    for customer in customers:
        bank.add_customer(customer)
        time.sleep(1)  # จำลองการมาถึงต่างเวลากัน
        
    print("\nแสดงลำดับคิว:")
    bank.display_queue()
    
    # จำลองการเรียกลูกค้าเข้ารับบริการ
    print("\nเริ่มเรียกลูกค้า:")
    for _ in range(len(customers)):
        bank.serve_next_customer()
        time.sleep(1)

if __name__ == "__main__":
    demo_bank_queue()
```
![image](https://github.com/user-attachments/assets/4f7086d1-abb4-454c-b9f6-6a656e029094)

![image](https://github.com/user-attachments/assets/55ce2a5c-b029-48e0-956b-71c4c0194c54)


