# Тестовое задание 
Этот описание содержит ответы на вопросы поставенные в тестовом задании:
Вопросы:
1."Как хорошо Вы владеете Python? Оцените свой уровень по шкале от 0 до 10" 
Если говорить о философии и о базовых библиотеках python то я бы оценил свои знания на 6 баллов знаю основные методы работы с типами и структурами данным, знаю объекто орентированное програмирование, однако питон очень большой(даже в рамках базовой библиотеки)поэтому не могу сказать что досконально занаю все методы и атрибуты базового языка програмирования, не говоря уже о огромном разнообразии странних библиотек.
2. "Насколько хорошо Вы знакомы с Google Sheets?"
Не могу сказать что обладаю огромными занаиями в google sheets. Я умею взаимодействовать с api и библиотекой от google.auth. Касательно гул таблиц умею строить графики и диаграмы,писать фукции, форматирвоать таблицы, знаком с фукционалом макросов,но совершенно не искущен в их написании поэтому оценю свои знания в 4 балла.
Задача 1:
Неизрасходованный бюджет = Изначальный бюджет - Суммарные расходы
Неизрасходованный бюджет = 40 - 0.9 * 40
Неизрасходованный бюджет = 40 - 36
Неизрасходованный бюджет = 4 доллара Ответ 4 доллара
Задача 2:
Задача 4:
Instagram и Youtube: Никто не использует оба сервиса.
Instagram и Facebook: Энн.
Youtube и Facebook: Джон.
Оба сервиса (Instagram и Youtube) и не использует Facebook: Никто.
Оба сервиса (Instagram и Facebook) и не использует Youtube: Энн.
Оба сервиса (Youtube и Facebook) и не использует Instagram: Джон.
Все три сервиса: Никто не использует все три сервиса.
Таким образом, у кого-то нет совпадающих предпочтений, но если рассматривать два сервиса, то у Энн и Джона есть совпадающие предпочтения (Instagram и Facebook).
## Техническая часть
1. Вычислите общую выручку за июль 2021 по тем сделкам, приход денежных
средств которых не просрочен.
```mask=df_cop["status"]=="Июль 2021"
mask_t=df_cop["status"]=="Август 2021"
index_July = df_cop.index[mask].tolist()[0]
index_August = df_cop.index[mask_t].tolist()[0]

df_selection = df_cop.loc[index_July+1:index_August-1]

df_selection["status"].value_counts()
answer_one=df_selection[df_selection.status=="ОПЛАЧЕНО"]["sum"].sum()
answer_one
```
### Ответ 859895.75
2. Как изменялась выручка компании за рассматриваемый период?
Проиллюстрируйте графиком.
```months=["Май","Июнь","Июль","Август","Сентябрь","Октябрь"]
income=[]
for i in range(len(months)):
    try:
        mask_one=df_cop["status"]==f"{months[i]} 2021"
        mask_two=df_cop["status"]==f"{months[i+1]} 2021"
        index_one = df_cop.index[mask_one].tolist()[0]
        index_two = df_cop.index[mask_two].tolist()[0]
        df_selection = df_cop.loc[index_one+1:index_two-1]
        gain=df_selection[df_selection.status=="ОПЛАЧЕНО"]["sum"].sum()
        income.append(gain)
    except:
        mask_one=df_cop["status"]==f"{months[-1]} 2021"
        index_one = df_cop.index[mask_one].tolist()[0]
        df_selection = df_cop.loc[index_one+1:]
        gain=df_selection[df_selection.status=="ОПЛАЧЕНО"]["sum"].sum()
        income.append(gain```
``` total_revenue = sum(income)

percentage_revenue = [(r / total_revenue) * 100 for r in income]

fig, ax1 = plt.subplots(figsize=(12, 6))

bars = ax1.bar(months, income, color='blue', label='Income')

x = np.arange(len(months))
coefficients = np.polyfit(x, income, 1)
trend_line = np.polyval(coefficients, x)

ax1.plot(months, trend_line, color='red', label='Линия тренда')

for bar, value in zip(bars, income):
    yval = bar.get_height()
    ax1.text(bar.get_x() + bar.get_width()/2, yval, f'{value:.2f}', ha='center', va='bottom')

ax2 = ax1.twinx()
ax2.set_ylabel('Процент от общей прибыли', color='green')
ax2.plot(months, percentage_revenue, color='green', marker='o', label='Процент от общей прибыли')

plt.title('Income и процент от общей прибыли по месяцам')
ax1.set_xlabel('Месяц')
ax1.set_ylabel('Выручка', color='blue')
ax1.legend(loc='upper left')
ax2.legend(loc='upper right')
plt.grid(axis='y')

plt.show()```
![image](https://github.com/Nikolairopin/Company_Test/assets/126417867/00afe3dd-b929-42f4-b858-71b872797e9d)
3 Кто из менеджеров привлек для компании больше всего денежных средств в
сентябре 2021?
```
mask=df_cop["status"]=="Сентябрь 2021"
mask_t=df_cop["status"]=="Октябрь 2021"
index_September = df_cop.index[mask].tolist()[0]
index_October = df_cop.index[mask_t].tolist()[0]

df_selection = df_cop.loc[index_September+1:index_October-1]
managers_score=df_selection.groupby("sale")["sum"].sum().sort_values(ascending=False)
answer_three=managers_score.index[0]
answer_three '''
### Ответ Смирнов
4. Какой тип сделок (новая/текущая) был преобладающим в октябре 2021?
'''mask=df_cop["status"]=="Октябрь 2021"
index_October = df_cop.index[mask].tolist()[0]

df_selection = df_cop.loc[index_October+1:]
answer_four=df_selection["new/current"].value_counts().sort_values(ascending=False).index[0]
answer_four```
### Ответ текущая
5 Сколько оригиналов договора по майским сделкам было получено в июне 2021?
```# функция для создания datetimeтипа данных в столбце receiving date 
def to_date_time(date):
    if pd.isna(date) or date == "-":
        return pd.NaT
    else:
        try:
            return pd.to_datetime(date, format="%d.%m.%Y")
        except ValueError:
            # If parsing fails, try with two-digit year
            return pd.to_datetime(date, format="%d.%m.%y")
 
df_cop["receiving_date"]=df_cop["receiving_date"].apply(to_date_time)
mask=df_cop["status"]=="Июнь 2021"
mask_t=df_cop["status"]=="Июль 2021"
index_June = df_cop.index[mask].tolist()[0]
index_July = df_cop.index[mask_t].tolist()[0]

df_selection = df_cop.loc[index_July+1:index_August-1]
df_selection=df_selection[df_selection["receiving_date"].dt.month==8]
df_selection=df_selection.dropna(subset=["receiving_date"])
answer_five=df_selection.count().iloc[0]
answer_five ``` 
### Ответ 81
## Финальное задание 
```mask_t=df_cop["status"]=="Июль 2021"
index_t = df_cop.index[mask_t].tolist()[0]

df_selection = df_cop.loc[2:index_t-1]
df_cur = df_selection[(df_selection["new/current"] == "текущая")&
                        (df_selection["status"] != "ПРОСРОЧЕНО")&
                        (df_selection["receiving_date"].dt.month >= 7) & 
                        (df_selection["receiving_date"].dt.day >= 1)]

df_new = df_selection[(df_selection["new/current"] == "новая") & 
                      (df_selection["status"] == "ОПЛАЧЕНО") & 
                      (df_selection["receiving_date"].dt.month >= 7) & 
                      (df_selection["receiving_date"].dt.day >= 1)]
df_new.loc[:, "residue"] = df_new["sum"] * 0.07
df_cur.loc[:, "residue"] = df_cur["sum"].apply(lambda x: x * 0.05 if x > 10000 else x * 0.03)
coct_table=pd.concat([df_new,df_cur],axis=0)

answer_final=coct_table.groupby("sale")["sum"].sum().sort_values(ascending=False)
answer_final```
### Ответ
Соколов,8973.700195
Васильев,30996.695312
Филимонова,63408.722656
Селиванов,93981.8125
Андреев,103276.304688
Кузнецова,141471.875
Иванов,149812.828125
Смирнов,178021.828125
Петрова,273685.0625


