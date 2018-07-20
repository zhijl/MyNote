# C 语言获取 Linux 平台时间

``` c
#include <time.h>
#include <stdio.h>

// 获取年月日，如需获取时分秒（p->tm_hour, p->tm_min, p->sec; 获取星期：p->tm_wday 0-6表示星期天、星期一...星期六）
void GetLocalTime(int* year, int* mon, int* day) {
  time_t timep;
  struct tm *p;
  time(&timep);
  p = gmtime(&timep);
  *year = p->tm_year + 1900;
  *mon = p->tm_mon + 1;
  *day = p->tm_mday;
}

int main(){
  int year, mon, day;
  GetLocalTime(&year, &mon, &day);
  printf("%d %d %d\n", year, mon, day);
  return 0;
}
```