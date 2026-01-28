# Bank-data-analysis
В данной работе представлен пет проект по аналитике данных банка с использованием Python и статистических методов.

Ссылка на датасет - https://archive.ics.uci.edu/dataset/222/bank+marketing

Датасет:
Португальский банк с мая 2008 года по февраль 2010 года проводил ряд обзвонов по свои клиентам с предложениями об открытии вклада. Решение клиента открыть вклад или не открыть записано в колонке 'y'. Также предоставлена информация об образовании клиентов, их возрасте, семейном положении, профессии, наличии кредита, истории звонков от банка и оформлял ли клиент вклад до этого.

Задача:
Изучить корреляцию различных данных о клиенте с его решением открыть вклад или нет. Построить графики конверсии и провести тесты с помощью статистических методов подтверждая гипотезы о зависимости или отклоняя их.
В проекте используется тест хи-квадрат, для количественной оценки силы и значимости связи между двумя категориальными переменными используется коэффициент V Крамера, а также используется метод доверительных интервалов.

Польза:
С помощью изучения этих данных можно понять стратегию работы с клиентами в будущем. Каким клиентам стоит предлагать открытие вклада в первую очередь из-за большой вероятности успеха, а кто скорее всего откажется.

Ход работы:
1. Считываем датасет
   ```python
   df = pd.read_csv(r'C:\Users\Vlad\Downloads\bank.csv', sep=';')
   df.head()
   ```
   ![Первые 5 строчек таблицы](images/bank_table.png)
2. Создаем функции для построения графиков
   <details>
   <summary>Гистрограмма + гистограмма с конверсией по оси y</summary>
   ```python
def plot_smart_bar(column, dataframe):

    plt.figure(figsize=(16, 5))

    # 1. График количества (Count Plot)
    plt.subplot(1, 2, 1)
    sns.countplot(data=dataframe, x=column, hue='y', palette='viridis')
    if column == 'age_group':
        plt.title('Количество клиентов по возрасту')
    elif column == 'education':
        plt.title('Количество клиентов по образованию')
    elif column == 'loan':
        plt.title('Количество клиентов с кредитом')
    elif column == 'housing':
        plt.title('Количество клиентов с кредитом на имущество')
    elif column == 'job':
        plt.title('Количество клиентов по профессии')
    else:
        plt.title(f'Количество клиентов по {column}')
    plt.ylabel('Количество')
    if column == 'age_group':
        plt.xlabel('Возраст')
    elif column == 'education':
        plt.xlabel('Образование')
    elif column == 'loan':
        plt.xlabel('Кредит')
    elif column == 'housing':
        plt.xlabel('Кредит на имущество')
    elif column == 'job':
        plt.xlabel('Профессия')
    plt.xticks(rotation=45)

    # 2. График вероятности (Conversion Rate)
    plt.subplot(1, 2, 2)
    temp_df = dataframe.copy()
    temp_df['y_numeric'] = temp_df['y'].map({'yes': 1, 'no': 0})
    
    sns.barplot(data=temp_df, x=column, y='y_numeric', palette='magma', errorbar=None)
    if column == 'age_group':
        plt.title('% успеха (конверсия) по возрасту')
    elif column == 'education':
        plt.title('% успеха (конверсия) по образованию')
    elif column == 'loan':
        plt.title('% успеха (конверсия) по кредиту')
    elif column == 'housing':
        plt.title('% успеха (конверсия) по кредиту на имущество')
    elif column == 'job':
        plt.title('% успеха (конверсия) по профессии')
    else:
        plt.title(f'% успеха (конверсия) по {column}')
    plt.ylabel('Conversion Rate')
    plt.xticks(rotation=45)

    plt.tight_layout()
    if column == 'age_group':
        plt.xlabel('Возраст')
    elif column == 'education':
        plt.xlabel('Образование')
    elif column == 'loan':
        plt.xlabel('Кредит')
    elif column == 'housing':
        plt.xlabel('Кредит на имущество')
    elif column == 'job':
        plt.xlabel('Профессия')
   ```

    plt.savefig(f'{column}.png')
    plt.show()
   ```
