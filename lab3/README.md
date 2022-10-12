# АНАЛИЗ ДАННЫХ И ИСКУССТВЕННЫЙ ИНТЕЛЛЕКТ [in GameDev]
Отчет по лабораторной работе #3 выполнил:
- Нечитайлов Никита Михайловчи
- ХПИ31
Отметка о выполнении заданий (заполняется студентом):

| Задание | Выполнение | Баллы |
| ------ | ------ | ------ |
| Задание 1 | * | 60 |
| Задание 2 | * | 20 |
| Задание 3 | * | 20 |

знак "*" - задание выполнено; знак "#" - задание не выполнено;

Работу проверили:
- к.т.н., доцент Денисов Д.В.
- к.э.н., доцент Панов М.А.
- ст. преп., Фадеев В.О.

## Цель работы
Познакомиться с программными средствами для создания системы машинного обучения и ее интеграции в Unity.

## Задание 1
### Реализовать систему машинного обучения в связке Python - Google-Sheets – Unity.

1.1 В проект unity добавим ml-agents-release_19/com.unity.ml-agents/package.json и ml-agents-release_19/com.unity.ml-agents.extensions/package.json

<img width="618" alt="image" src="https://user-images.githubusercontent.com/87676077/194763287-ed791174-d5a2-4eda-a634-acf936acd36b.png">

1.2 Создадим виртуальное окружение и скачаем в него mlagents 0.28.0 и torch 1.7.1

<img width="205" alt="image" src="https://user-images.githubusercontent.com/87676077/194763438-9ef13c77-6aaf-43f0-803d-6dcee4f65ab3.png">

1.3 Создим на сцене плоскость, шар и куб и изменим их цвета

![image](https://user-images.githubusercontent.com/94524541/195383259-7c19e8da-17c6-42c0-9076-c689dd6cd671.png)

1.4 Добавим сфере скрипт RollerAgent.cs

```cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using Unity.MLAgents;
using Unity.MLAgents.Sensors;
using Unity.MLAgents.Actuators;

public class RollerAgent : Agent
{
    Rigidbody rBody;
    // Start is called before the first frame update
    void Start()
    {
        rBody = GetComponent<Rigidbody>();
    }

    public Transform Target;
    public override void OnEpisodeBegin()
    {
        if (this.transform.localPosition.y < 0)
        {
            this.rBody.angularVelocity = Vector3.zero;
            this.rBody.velocity = Vector3.zero;
            this.transform.localPosition = new Vector3(0, 0.5f, 0);
        }

        Target.localPosition = new Vector3(Random.value * 8-4, 0.5f, Random.value * 8-4);
    }
    public override void CollectObservations(VectorSensor sensor)
    {
        sensor.AddObservation(Target.localPosition);
        sensor.AddObservation(this.transform.localPosition);
        sensor.AddObservation(rBody.velocity.x);
        sensor.AddObservation(rBody.velocity.z);
    }
    public float forceMultiplier = 10;
    public override void OnActionReceived(ActionBuffers actionBuffers)
    {
        Vector3 controlSignal = Vector3.zero;
        controlSignal.x = actionBuffers.ContinuousActions[0];
        controlSignal.z = actionBuffers.ContinuousActions[1];
        rBody.AddForce(controlSignal * forceMultiplier);

        float distanceToTarget = Vector3.Distance(this.transform.localPosition, Target.localPosition);

        if(distanceToTarget < 1.42f)
        {
            SetReward(1.0f);
            EndEpisode();
        }
        else if (this.transform.localPosition.y < 0)
        {
            EndEpisode();
        }
    }
}
```

1.5 Добавим сфере Decision Requester и Behavior Parameters

<img width="232" alt="image" src="https://user-images.githubusercontent.com/87676077/194763615-6b985fc3-a1c7-4b06-bbcb-0e8b83df3d97.png">

1.6 В корень проекта добавим файл конфигурации нейронной сети и запустим работу ml-агена

![image](https://user-images.githubusercontent.com/94524541/195384687-ba578d53-f1b7-43af-b01d-f75ebb84e419.png)

1.7 Сделаем несколько копий модели TargetArea, и обучим их

![image](https://user-images.githubusercontent.com/94524541/195383859-cfe709f2-87bf-47a0-8e88-081817176de9.png)


1.8 Проверим работу полученной модели

<img width="236" alt="image" src="https://user-images.githubusercontent.com/87676077/194764102-50b73fc2-5b3a-4d35-b400-be180d0b177a.png">

27 копий

![bandicam 2022-10-12 15-33-49-264](https://user-images.githubusercontent.com/94524541/195384200-1aa65ff9-988a-4ed2-82e3-5a49f7b6beb0.gif)

## Вывод по первому заданию 
При увеличении количества копий, модель обучается быстрее.

## Задание 2
### Подробно описать каждую строку файла конфигурации нейронной сети. Самостоятельно найти информацию о компонентах Decision Requester, Behavior Parameters, добавленных сфере.

```yaml
behaviors:
  RollerBall: # указываем id агента
    trainer_type: ppo # режим обучения (Proximal Policy Optimization)
    hyperparameters:
      batch_size: 10 # количество опытов на каждой итерации
      buffer_size: 100 # количество опыта, которое нужно набрать перед обновлением модели
      learning_rate: 3.0e-4 # начальная скорость обучения
      beta: 5.0e-4 # сила регуляции энтропии, увеличивает случайность действий
      epsilon: 0.2 # порог расхождений между старой и новой политиками при обновлении
      lambd: 0.99 # параметр регуляции, насколько сильно агент полагается на текущий value estimate
      num_epoch: 3 # количество проходов через буфер опыта, при выполнении оптимизации
      learning_rate_schedule: linear # определяет как скорость обучения изменяется с течением времени
                                     # linear линейно уменьшает скорость
    network_settings:
      normalize: false # отключаем нормализацию входных данных
      hidden_units: 128 # количество нейронов в скрытых слоях сети
      num_layers: 2 # количество скрытых слоёв в сети
    reward_signals:
      extrinsic:
        gamma: 0.99 # коэффициент скидки для будущих вознаграждений
        strength: 1.0 # коэффициент на который умножается вознаграждение
    max_steps: 500000 # общее количество шагов, которые должны быть выполнены в среде до завершения обучения
    time_horizon: 64 # сколько опыта нужно собрать для каждого агента, прежде чем добавлять его в буфер
    summary_freq: 10000 # количество опыта, который необходимо собрать перед созданием и отображением статистики
```

`Decision Requester` - запрашивает решение через регулярные промежутки времени.

`Behavior Parameters` - определяет принятие объектом решений, в него указывается какой тип поведения будет использоваться: уже обученная модель или удалённый процесс обучения.

## Задание 3
### Доработать сцену и обучить ML-Agent таким образом, чтобы шар перемещался между двумя кубами разного цвета. Кубы должны, как и впервом задании, случайно изменять кооринаты на плоскости.

3.1 Добавим второй куб, создадим для него цвет

![image](https://user-images.githubusercontent.com/94524541/195385499-90272cb9-4e47-460c-a03d-4003e0417d1a.png)

3.2 необходимо изменить c# код для двух таргетов

``` cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using Unity.MLAgents;
using Unity.MLAgents.Sensors;
using Unity.MLAgents.Actuators;

public class RollerAgent : Agent
{
    Rigidbody rBody;
    // Start is called before the first frame update
    void Start()
    {
        rBody = GetComponent<Rigidbody>();
    }

    public GameObject Target1;
    public GameObject Target2;
    private bool target1Collected;
    private bool target2Collected;
    public override void OnEpisodeBegin()
    {
          if (this.transform.localPosition.y < 0)
    {
        this.rBody.angularVelocity = Vector3.zero;
        this.rBody.velocity = Vector3.zero;
        this.transform.localPosition = new Vector3(0, 0.5f, 0);
    }

    Target1.transform.localPosition = new Vector3(Random.value * 8-4, 0.5f, Random.value * 8-4);
    Target2.transform.localPosition = new Vector3(Random.value * 8-4, 0.5f, Random.value * 8-4);
    Target1.SetActive(true);
    Target2.SetActive(true);
    target1Collected = false;
    target2Collected = false;
    }
    public override void CollectObservations(VectorSensor sensor)
    {
         sensor.AddObservation(Target1.transform.localPosition);
    sensor.AddObservation(Target2.transform.localPosition);
    sensor.AddObservation(this.transform.localPosition);
    sensor.AddObservation(target1Collected);
    sensor.AddObservation(target2Collected);
    sensor.AddObservation(rBody.velocity.x);
    sensor.AddObservation(rBody.velocity.z);
    }
    public float forceMultiplier = 10;
    public override void OnActionReceived(ActionBuffers actionBuffers)
    {Vector3 controlSignal = Vector3.zero;
    controlSignal.x = actionBuffers.ContinuousActions[0];
    controlSignal.z = actionBuffers.ContinuousActions[1];
    rBody.AddForce(controlSignal * forceMultiplier);

    float distanceToTarget1 = Vector3.Distance(this.transform.localPosition, Target1.transform.localPosition);
    float distanceToTarget2 = Vector3.Distance(this.transform.localPosition, Target2.transform.localPosition);

    if (!target1Collected & distanceToTarget1 < 1.42f)
    {
        target1Collected = true;
        Target1.SetActive(false);
    }

    if (!target2Collected & distanceToTarget2 < 1.42f)
    {
        target2Collected = true;
        Target2.SetActive(false);
    }

    if(target1Collected & target2Collected)
    {
        SetReward(1.0f);
        EndEpisode();
    }
    else if (this.transform.localPosition.y < 0)
    {
        EndEpisode();
    }
}
}
```
3.3 Создаем так же несколько моделей, и обучаем их

![image](https://user-images.githubusercontent.com/94524541/195386305-9216760e-0026-4381-9b61-efefe4ecc241.png)

3.4 Получаем результат 

![bandicam 2022-10-12 18-11-29-336](https://user-images.githubusercontent.com/94524541/195386830-e9cc7d6b-b92d-40cc-9fa4-df48c7b02f9c.gif)

## Выводы

Я научился работать с ML агентом, понял как написать код для обучения ИИ, понял как получить результат обучения. Что значат данные в yaml. Понял, что чем больше моделей задействованы в обучении, тем лучше будет результат обучения. Разобрался как создать пространство под агента, и как пользоваться консолью в anaconda.

## Powered by

**BigDigital Team: Denisov | Fadeev | Panov**
