## Вывод файла в формате Excel
```bash
pip install XlsxWriter
```
```python
import io
import xlsxwriter

@action(methods=["GET"], detail=True, url_path="products/get_xlsx")
    def get_xlsx(self, request, pk, *args, **kwargs):
        qs = self.get_object().products.all()
        ser = serializers.ProductsStatisticsSerializer(qs, many=True)

        output = io.BytesIO()

        workbook = xlsxwriter.Workbook(output)
        worksheet = workbook.add_worksheet()

        worksheet.write_row(0, 0, ser.data[0].keys())
        for idx, line in enumerate(ser.data):
            data = line.values()
            worksheet.write_row(idx+1, 0, data)

        workbook.close()

        output.seek(0)

        filename = 'django_simple.xlsx'
        response = HttpResponse(
            output,
            content_type='application/vnd.openxmlformats-officedocument.spreadsheetml.sheet'
        )
        response['Content-Disposition'] = 'attachment; filename=%s' % filename

        return response
```
## Вывод файла в формате CSV

```python
import csv

@action(methods=["GET"], detail=True, url_path="products/get_csv")
def get_csv(self, request, pk, *args, **kwargs):
    qs = self.get_object().products.all()
    ser = serializers.ProductsStatisticsSerializer(qs, many=True)
    response = HttpResponse(content_type="text/csv")
    response['Content-Disposition'] = 'attachment; filename="products.csv"'
    writer = csv.writer(response)
    writer.writerow(ser.data[0].keys())
    for line in ser.data:
        data = line.values()
        writer.writerow(data)
    return response
```
