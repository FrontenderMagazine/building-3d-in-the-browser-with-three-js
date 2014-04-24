# 3D-моделирование в браузере с помощью three.js

Сегодня браузеры предоставляют нам возможности, о которых мы не могли и мечтать
5 или 10 лет назад. Используя ресурсы браузера, можно создавать целые игровые 
3D-миры и интерактивные сайты, обеспечивающие эффект присутствия.

Сейчас, когда одна из социальных сетей приобрела компанию, работающую в сфере 
разработки виртуальной реальности, самое время начать работать с 3D. 
Поразительно, но теперь мы можем создавать 3D-модели с помощью JavaScript, 
работая с 3D-сетями и материалами.

Конечно, несмотря на то, что все это возможно, 3D-моделирование требует 
написания большого количества кода. И вот здесь three.js в работу включается, 
позволяя нам создавать 3D-окружения просто и с помощью меньшего количества кода.

## Совместимость с браузерами

К сожалению, будучи новой технологией, 3D поддерживается не всеми браузерами. 
В настоящее время мы ограничены браузерами Chrome, Safari и Firefox.

Пройдет время, и поддержка технологии со стороны IE станет лучше, но в настоящее 
время вам потребуется запасной вариант решения для поклонников Microsoft.

## Приступаем к работе

Первым делом перейдите на [сайт three.js][1] и скачайте последнюю версию 
библиотеки.

Затем подключите библиотеку в секции `head` вашего документа, как вы делаете
это со всеми остальными JavaScript-файлами — и все готово.

## Создаем первую сцену

Чтобы отрендерить что-либо с помощью three.js, сначала необходимо создать сцену, 
добавить камеру и настроить рендерер. Начнем со сцены:

	var scene = new THREE.Scene();

Теперь нам нужна камера. Представьте, что камера — это точка, с которой 
пользователь смотрит на сцену. Three.js предоставляет широкие возможности 
настройки камеры, но для простоты мы будем использовать перспективную камеру:

	var camera = new THREE.PerspectiveCamera(100, window.innerWidth / window.innerHeight, 0.1, 1000);

Этот метод принимает 4 параметра: первый — это вертикальный угол обзора, 
второй — соотношение сторон кадра (я рекомендую всегда ориентироваться на 
видимую область документа ([вьюпорт][4] — *прим. переводчика*)), третий — 
расстояние до передней плоскости отсечения и, наконец, четвертый — расстояние 
до задней плоскости отсечения. Последние два параметра определяют ограничения 
области рендеринга, чтобы объекты, расположенные слишком близко или слишком 
далеко, не отрисовывались, потому что это вызвало бы излишнее потребление 
ресурсов.

После этого нам необходимо настроить WebGL-рендерер:

	var renderer = new THREE.WebGLRenderer(); 
	renderer.setSize( window.innerWidth, window.innerHeight ); 
	document.body.appendChild( renderer.domElement );

Как вы видите, сначала нам необходимо создать объект рендерера, затем установить 
его размер в соответствии с размером видимой области документа и, наконец, 
добавить его на страницу, чтобы создать пустой элемент `canvas`, с которым мы 
будем работать.

## Добавляем объекты

Теперь, когда мы настроили сцену, мы создадим наш первый 3D-объект.

Пусть для примера это будет обычный цилиндр:

	var geometry = new THREE.CylinderGeometry( 5, 5, 20, 32 );

Этот метод также принимает 4 параметра: первый — это радиус верхнего основания 
цилиндра, второй — это радиус нижнего основания цилиндра, третий — это высота, 
последний — это количество образующих цилиндр вертикальных сегментов.

Мы математически описали объект, теперь нам необходимо обернуть его материалом,
так чтобы он имел визуальное представление на экране:

	var material = new THREE.MeshBasicMaterial( { color: 0x0000ff, wireframe: true} );

Этот код описывает материал, из которого «изготовлен» объект, в данном случае
он синего цвета. Я установил параметр `wireframe` в значение `true`, потому что 
так объект будет выглядеть более четким при анимации.

Наконец, нам необходимо добавить наш цилиндр в сцену, примерно так:

	var cylinder = new THREE.Mesh( geometry, material ); 
	scene.add( cylinder );

Последнее, что нам необходимо сделать перед тем, как отрендерить сцену, это 
настроить положение камеры:

	camera.position.z = 20;

Как вы видите, мы используем достаточно простой JavaScript-код, потому что
всю сложную работу выполняет three.js, и нам не нужно о ней беспокоиться.

## Рендеринг сцены

Если вы запустите полученный пример в браузере, вы увидите, что ничего не 
происходит. Это потому что мы не дали команду на рендеринг сцены. Чтобы 
отрендерить сцену, необходимо выполнить следующий код:

	function render() {
		requestAnimationFrame(render);
		renderer.render(scene, camera);
	}
	render();

Если вы еще раз заглянете в браузер, вы увидите в окне наш цилиндр, но вряд ли
скажете, что это впечатляет.

Чтобы действительно прочувствовать особенность 3D, необходимо анимировать объект,
что можно сделать с помощью пары небольших изменений в функции `render`:

	function render() {
		requestAnimationFrame(render);
		cylinder.rotation.z += 0.01;
		cylinder.rotation.y += 0.1;
		renderer.render(scene, camera);
	}
	render();

Если вы снова откроете браузер, вы увидите анимированный в 3D цилиндр.

## Заключение

Если вы хотите посмотреть демонстрационный пример и поэкспериментировать с 
кодом, вы можете [сделать это здесь][2].

Как видите, для создания такой (впрочем, очень простой) сцены потребовалось 
менее пары десятков строк кода, и не потребовалось реализовывать сложную
математическую модель.

Если вы обратите внимание на [примеры][3] на сайте three.js, вы найдете
совершенно невероятные выполненные работы.

Эта удивительная JavaScript-библиотека действительно снизила порог входа в 
3D-моделирование до уровня, доступного каждому, кто владеет основами работы 
с JavaScript.

[1]: http://threejs.org/
[2]: http://codepen.io/SaraVieira/pen/Ctnov
[3]: http://threejs.org/examples/
[4]: https://github.com/web-standards-ru/dictionary/blob/master/Dictionary.md#v