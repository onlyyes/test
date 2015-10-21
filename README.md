# test
test
1) Проверка Package/FormParcels
  1.1) для метода 1, 2 и 3 ("Package/FormParcels".Attributes["Method"])  у каждого Package/FormParcels/NewParcel проверяется PrevCadastralNumbers/CadastralNumber.
  Если CadastralNumber отсутствуют, то шибка "Некорректное значение элемента " + "Package/FormParcels".Attributes["Method"]"
  Если CadastralNumber больше 1 у NewParcel, то ошибка "Некорректное значение элемента " + "Package/FormParcels".Attributes["Method"]"
  Если CadastralNumber пустое, то ошибка
  CadastralNumber у всех NewParcel должны совпадать, иначе ошибка.
  
  1.2) для метода 5 ("Package/FormParcels".Attributes["Method"]) 
  у каждого "Package/FormParcels/NewParcel" проверяем "PrevCadastralNumbers/CadastralNumber". 
  Если элемент присутствует, то шибка "При таком способе образования элемент " + ... + " должен отсутствовать";
  
  1.3) для метода 6 ("Package/FormParcels".Attributes["Method"]) 
  у всех элементов "Package/FormParcels/NewParcel" должен присутствовать "PrevCadastralNumbers/CadastralNumber"
  
2) Проверка каждого "Package/FormParcels/SpecifyRelatedParcel". Для проверки выбирается "FormParcels"
  2.1) Если присутствует аттрибут "NumberRecord", то значит у нас контур
    2.1.1) Если присутствует "Contours", то ошибка "Элемент не может присутствовать для контура"
    2.1.2) Если присутствует "ExistSubParcels", то ошибка "Элемент не может присутствовать для контура"
  2.2) Если отсутствует (или пустой) аттрибут "NumberRecord", то у нас ЗУ
    2.2.1) Если присутствует "DeleteAllBorder", то ошибка "Элемент может присутствовать только для контура"
    
// Если в блоке присутствуют только OldOrdinate, то должны совпадать первая и последняя точка – удаление внутреннего контура (дырки).
// Если в блоке присутствуют только NewOrdinate, то должны совпадать первая и последняя точка – добавление внутреннего контура (дырки).
  
  2.3) Определение того, что будет происходить с объектом. У всех объектов ChangeBorder проверяются теги "ChangeBorder\OldOrdinate" и "ChangeBorder\NewOrdinate". Если оба тега есть, то это изменение контура, если есть только OldOrdinate - то это удаление, если есть только NewOrdinate - то это создание
    2.3.1) Если создание. У элемента "ChangeBorder" сравниваем первую и последнюю пары координат у "NewOrdinate". Если они не совпадают, то ошибка "В элементах /ChangeBorder присутствуют только NewOrdinate (добавление внутреннего контура). Должны совпадать первая и последняя точка"
    2.3.2) Аналогично 2.3.1, но координаты проверяются у OldOrdinate
3) Проверка каждого "Package/SpecifyParcel/SpecifyRelatedParcel" Для проверки выбирается "SpecifyParcel"
  Проверки совпадают с пунктом 2)
4) Проверяется каждый элемент "Package/FormParcels/NewParcel".
  4.1) Выбирается элемент "EntitySpatial". Для каждого элемента "SpatialElement" Выполняется:
    4.1.1) Для каждого "SpelementUnit" из элемента "NewOrdinate" или "Ordinate". Выбирается первая и последняя точка. Если они не равны, то ошибка "Контур должен быть замкнут"
  4.2) Выбриается элемент "AllBorder/EntitySpatial". Для каждого элемента "SpatialElement" Выполняется:
    4.2.1) Аналогично 4.1.1
  4.3) Выбираются ЧЗУ. Элемент "SubParcels".
  4.4) Для каждого ЧЗУ (если они есть) выполняется:
    4.4.1) Если имя тэга ЧЗУ "NewSubParcel" или "ExistSubParcel" то выбирается элемент "EntitySpatial" и для него выполняются пункт 4.1.1. Далее выполянется
      4.4.1.1) Если есть элемент "Contours", то выбирается элемент "Contours/Contour", для него выбирается "EntitySpatial" и для него выполняется пункт 4.1.1
  4.5) Выбриаются элементы "Contours". Для Каждого проверяется имя. Если оно "NewContour" или "ExistContour" или "Contour", то выбриается элемент "EntitySpatial" и выполняется пункт 4.1.1
5) Проверяется каждый элемент "Package/FormParcels/SpecifyRelatedParcel". Проверка происходит как и для пункта 4, но в пункте 4.3 нужно выбрать "ExistSubParcels"
6) Проверяется каждый элемент "Package/FormParcels/SpecifyParcelApproximal" //В функцию передается "FormParcels/SpecifyParcelApproximal"
  6.1) Для каждого элемента "ExistParcel" выполяняются пункты 4.1 - 4.5
  6.2) Для каждого элемента "ExistEZ/ExistEZParcels" выполяняются пункты 4.1 - 4.5
  6.3) Для каждого элемента "ExistEZ/ExistEZParcels" выполняется
    6.3.1)  Для каждого элемента "CompositionEZ/InsertEntryParcels/InsertEntryParcel" выполняется
      6.3.1.1) Если есть элемент "NewEntryParcel", то для него выполняются пункты 4.1 - 4.5
  6.4) Для каждого элемента "ExistEZ/ExistEZEntryParcels/ExistEZEntryParcel" выполняются пункты 4.1 - 4.5
7) Для каждого элемента "Package/SpecifyParcel" выполняются пункты 6.1 - 6.4
8) Для каждого элемента "Package/SpecifyParcel/SpecifyRelatedParcel" Выполняются проверки из пункта 5
9) Для каждого элемента "Package/SpecifyParcel/SpecifyParcelApproximal" выполяются пункты 6.1 - 6.4
10) Для каждого элемента "Package/SpecifyParcelsApproximal/SpecifyParcelApproximal" выполяются пункты 6.1 - 6.4
11) Для каждого элемента "Package/SubParcels/NewSubParcel" Выполняются пункты 4.1 - 4.5
12) Для каждого "Package/SubParcels/ExistSubParcel" выполняются пункты 4.1 - 4.5

Продолжить с // 7. Элемент  AppliedFile///////////////////////////////////////////////////////////////////
