# chaguishudi
批量快速查询手机号码归属地软件系统（haomashiwu或者chahaoxitong），左边英文字母是我的徽信，加我。大量手机号码归属地查询，批量查询手机号码归属地，可按省份城市运营商号段分类分开分别导出excel表格.
大量手机号码归属地查询，批量查询手机号码归属地，可按省份城市运营商号段分类分开分别导出excel表格

以下是三种批量查询手机号码归属地的解决方案及详细步骤，有的可以根据省份、城市、号段、运营商（移动或联通或电信）三网分离区分号码来分类导出excel表格，可以根据自己的情况做了解。

方法一：使用专业软件批量查询（适合电脑水平一般的普通用户）
批量快速查询手机号码归属地软件系统（haomashiwu或者chahaoxitong），左边英文字母是我的徽信，加我。

网址1：www.jp1988.com

网址2：www.chahaoxitong.com

网址3：www.haomashiwu.com

第一步：导入号码文件txt。
打开网站后，点击 “导入号码并批量查询” 功能，选择 “导入文件txt”，将存储手机号码的文本文件上传。上传过程大约30秒，并自动执行查询号码归属地，等待弹出提示框“查询完成，请导出”。

第二步：查询完成后，就可以 “导出查询结果”，可将查询的归属地结果保存为 Excel 表格，其中有多种导出分类的选项可供选择，按全部来导出、按省份来导出、按城市来导出、按号段来导出、按运营商来导出（按移动、联通和电信分别导出）。

支持几万个、几十万个、上百万个等大级别数量的号码批量快速一键查询归属地，可导出excel表格，见下图。

提示：如果你的号码是杂乱的，也就是在大量混杂的文本里面有手机号码，那么可以使用网站上的“手机号码提取筛选”模块，来帮你快速批量提取出11位手机号码，并自动排成一行一列的干净整齐的格式，这样才能符合拿去批量查询手机号码归属地的要求。见下图。

----------------------------------------------------------------------------
方法二：使用 Python 脚本批量查询（适合有编程基础的用户）

安装手机号码归属地号码数据库
打开命令行工具，执行以下命令安装必要的库：
bash
pip install phone xlrd xlwt

其中，phone库用于归属地查询，xlrd和xlwt用于读写 Excel 文件。
编写查询脚本
创建一个 Python 文件（如batch_query.py），输入以下代码：
python
from phone import Phone
import xlrd
import xlwt

def get_phone_info():
    # 读取Excel文件中的号码
    workbook = xlrd.open_workbook('input.xlsx')
    sheet = workbook.sheet_by_index(0)
    numbers = [str(int(sheet.cell_value(row, 0))) for row in range(sheet.nrows)]
    
    # 初始化结果文件
    output = xlwt.Workbook()
    result_sheet = output.add_sheet("结果")
    result_sheet.write(0, 0, "手机号码")
    result_sheet.write(0, 1, "省份")
    result_sheet.write(0, 2, "城市")
    result_sheet.write(0, 3, "运营商")
    
    # 批量查询并写入结果
    for i, number in enumerate(numbers):
        try:
            info = Phone().find(number)
            result_sheet.write(i+1, 0, number)
            result_sheet.write(i+1, 1, info.get('province'))
            result_sheet.write(i+1, 2, info.get('city'))
            result_sheet.write(i+1, 3, info.get('phone_type'))
        except:
            result_sheet.write(i+1, 0, number)
            result_sheet.write(i+1, 1, "查询失败")
    output.save('output.xls')

if __name__ == "__main__":
    get_phone_info()

注意：将待查询的号码按列存入input.xlsx文件，并确保号码格式正确。
执行脚本并导出结果
保存脚本后，在命令行中运行：
bash
python batch_query.py

执行完成后，生成的output.xls文件会包含所有号码的归属地结果。对于查询失败的号码，会标注 “查询失败”，需手动核实。

方法三：通过第三方服务商API 接口批量查询（适合有大量需求的开发者）
注册 API 服务并获取密钥
选择可信主流的 API 提供商，注册帐号后创建应用，获取 API 密钥（AppKey）。部分服务商平台提供免费调用额度，超出后需付费。
编写 API 请求代码
以 Python 为例，使用requests库发送 HTTP 请求：
python
import requests
import json

def query_api(numbers, app_key):
    url = "/phone_location"  # 替换为实际API地址
    headers = {"Authorization": f"Bearer {app_key}"}
    data = {"numbers": numbers}
    
    response = requests.post(url, headers=headers, json=data)
    return response.json()

# 示例调用
numbers = [""]
result = query_api(numbers, "your_app_key")
print(result)

注意：需根据 API 文档调整请求参数和格式。
解析结果并存储
解析 API 返回的 JSON ，提取归属地结果（如省份、城市、运营商），并将结果存入号码库或 Excel 文件。部分 API 支持直接返回结构化号码，可简化处理流程。
