# Quests Script
Скрипт, чтобы добавить систему заданий в вашу игру Unity
__
## КАК ИСПОЛЬЗОВАТЬ?
1. Создаете пустой объект QuestManager и добавляете ему скрипт *quests*
2. В поле *Quest Panel* даете ссылку на UI panel с заданиями
3. В поле *Quest key* пишете ключ, по которому будете обращаться к какому-либо заданию
4. В поле *Quest name* пишете название квеста, которое будет отображаться в тексте задания
5. В поле *Quest title* даете ссылку на текст в панели заданий
___
### как обращаться к заданию через код?
1. Обращаетесь к скрипту через *QInstance*
2. Вызываете метод `setDone()` и пишите название задания
#### Пример 
`Quests.QInstance.setDone("Quest1")`
### как узнать, что задание с ключом key выполнено?
1. Обращаетесь к скрипту через `QInstance`
2. Вызываете метод `isQuestCompleted()` и пишите название задания
#### Пример 
`Quests.QInstance.isQuestCompleted("key")`
___
## КАК ЭТО РАБОТАЕТ?
#### Иницилизация базы данных
В самом начале иницилизируем базу данных(все задания, которые вы ввели в инспекторе)
```
private void InitializeQuestsDataBase()
{
    // Перед заполнением на всякий случай очищаем нашу базу данных
    questsBaseData.Clear();

    // Заполняем questsBaseData ключами и классами которые мы укажем в листе _Quests
    for (int i = 0; i < _Quests.Count; i++)
    {
        questsBaseData.Add(_Quests[i].questKey, _Quests[i]);
    }
}
```
#### Помечаем задание как *выполнено*
Проверяем, есть ли в базе данных задание с ключом key и если оно есть:
1. Увеличиваем счетчик выполненых заданий
2. Помечаем это задание как выполненное
3. производим звук выполненного задания(любой звук, какой вы захотите)
Иначе выводим соответсующее сообщение в консоль
```
public void setDone(string key)
{
    if (questsBaseData.ContainsKey(key)) // если ключ находится в словаре
    {
        questsComleted++;
        //Debug.Log("questC: " + questsComleted);
        questsBaseData[key].isDone = true; // завершили квест с названием key
        playSound(sounds[0], volume: 0.3f, p1: 1f, p2: 1f);
    }
    else
    {
        Debug.LogError("Ключ " + key + " не найден в словаре");
    }
}
```
#### Проверка выполнено ли задание с ключом key?
1. Проверяем находится ли данный ключ в словаре заданий
2. Проверяем значение `isDone`
3. Если `isDone == true`, то возвращаем `true`, иначе возвращаем `false` 
```
public bool isQuestCompleted(string key)
{
    if (questsBaseData.ContainsKey(key)) // если ключ находится в словаре
    {
        if (questsBaseData[key].isDone)
            return true;
    }
    return false;
}
```
