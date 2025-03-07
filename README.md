# Sugar Scope
# 控糖助手 iOS 终极蓝图

[!TIP]
**立即行动建议**：  
1. 从 Phase1 的 Realm 数据建模开始（可使用下方代码片段）  
2. 申请 [Open Food Facts API 开发者权限](https://world.openfoodfacts.org/data) 补充中国食品数据  

---

## 快速启动代码片段

### Realm 数据模型示例
```swift
// 预装食品模型
class LocalFood: Object {
    @Persisted var barcode: String?  // 包装食品条码
    @Persisted var name: String
    @Persisted var sugarAdded: Double  // 每100克添加糖含量
    @Persisted var category: String    // 类别：饮料/零食等
}

// 用户扫描记录
class ScanRecord: Object {
    @Persisted(primaryKey: true) var id: ObjectId
    @Persisted var date: Date
    @Persisted var sugarValue: Double
    @Persisted var food: LocalFood?
}

# CSV output suggestion
``` swift
func exportCSV(filter: ExportFilter) -> URL? {
    let records = realm.objects(ScanRecord.self)
        .filter("date BETWEEN {%@, %@}", filter.startDate, filter.endDate)
    
    let csvString = records.map { record in
        "\(record.date),\(record.food?.name ?? "自定义"),\(record.sugarValue)g"
    }.joined(separator: "\n")
    
    return saveToTempFile(csvString) // 实现文件存储
}
