# Petrunina.

# UI. Создание главной формы для игры и элементов управления
Начнём написание нашей игры с создания нового проекта на языке C# с типом "Приложение Windows Forms (.NET Framework)". Имя проекта выбираем TicTacToe (так в переводе звучит название игры "крестики-нолики").

![csharp-tic-tac-toe-1](https://github.com/user-attachments/assets/86803649-60ba-41c6-841d-e1732610e133)

У нас готова главная форма, и сейчас нам нужно создать игровое поле, где мы и будем играть в "крестики-нолики".

![csharp-tic-tac-toe-3](https://github.com/user-attachments/assets/7a95ae8a-33eb-450a-93a5-61ee90ea8bdc)

Для этого нового элемента Panel установите следующие свойства:

BackColor - Indigo (при выборе цвета используйте готовое значение цвета на вкладке "Интернет")
Location - 12; 12
Size - 99; 96
Name - panelCell0_0
Как нетрудно догадаться, это прототип клетки для нашего игрового поля, куда мы в дальнейшем будем ставить "крестик" или "нолик". Теперь нам нужно скопировать этот элемент 8 раз (т.е. общее количество элементов Panel должно выйти 9 - по три в каждом ряду). Для копирования панелей используйте комбинации Ctrl+C, Ctrl+V или воспользуйтесь пунктами "Копировать", "Вставить" в контекстном меню для элемента.

У вас должна выйти примерно следующая картина:

![csharp-tic-tac-toe-4](https://github.com/user-attachments/assets/f27039b9-1995-4199-b33a-fcda3a57cef1)

UI. Программирование интерфейса и событий на главной форме
В этой части статьи мы запрограммируем базовое поведение размещённых элементов формы. Например, мы сделаем так, чтобы при наведении курсора на кнопки происходил эффект смены цвета фона для кнопки, а при выводе курсора из области кнопки - цвет фона возвращался к первоначальному.

Кнопка выхода из игры
Сначала мы запрограммируем поведение кнопки выхода - ведь она нам сразу пригодится для тестирования внешнего вида и поведения нашей формы, чтобы мы без труда могли завершить программу.

Выберите панель с именем panelCloseButton и в окне "Свойства" нажмите на иконку "молнии" - это раздел "События" для данной панели. Сделайте по двойному клику напротив таких событий как Click, MouseEnter, MouseLeave, чтобы для каждого из них сгенерировались соответствующие методы-обработчики.

Теперь выберите метку с именем labelClose и таким же образом сгенерируйте для неё методы-обработчики для событий Click и MouseEnter.

Далее создадим в коде главной формы отдельный метод ButtonMouseEnter(Panel buttonPanel) и заполним сгенерированные методы следующим образом:
private void ButtonMouseEnter(Panel buttonPanel) {
            buttonPanel.BackColor = Color.Purple;
            Cursor = Cursors.Hand;
        }        

        private void panelCloseButton_Click(object sender, EventArgs e) {
            Application.Exit();
        }

        private void panelCloseButton_MouseEnter(object sender, EventArgs e) {
            ButtonMouseEnter(panelCloseButton);
        }

        private void panelCloseButton_MouseLeave(object sender, EventArgs e) {
            panelCloseButton.BackColor = Color.Indigo;
            Cursor = Cursors.Default;
        }

        private void labelClose_Click(object sender, EventArgs e) {
            Application.Exit();
        }

        private void labelClose_MouseEnter(object sender, EventArgs e) {
            ButtonMouseEnter(panelCloseButton);
        }

2
Обработчики panelCloseButton_Click и labelClose_Click выполняют одно и то же действие Application.Exit(), что позволяет завершить игру, независимо от того, в какой именно части панели мы кликнули - на метке или на самой области панели.

Метод ButtonMouseEnter нужен для того, чтобы поменять цвет фона панели для кнопки выхода и поменять стандартный курсор "стрелка" на курсор "руки". Его мы вызываем при введении курсора мыши либо в область панели для кнопки, либо в область самой метки с надписью "X".

В событии panelCloseButton_MouseLeave мы возвращаем курсор обратно на "стрелку" и возвращаем цвет фона на первоначальный - Color.Indigo.

Можете теперь запустить приложение и протестировать форму в действии. Сейчас вы должны заметить эффект при наведении курсора на кнопку выхода из игры. Попробуйте нажать на кнопку - приложение должно завершить работу.

Программируем эффект наведения курсора на клетки игрового поля
Теперь сделаем так, чтобы при наведении курсора на клетки игрового поля менялся цвет соответствующей клетки. Это придаст немного интерактивности игровому процессу при прохождении курсора мыши над клетками игрового поля.

Для этого по очереди выделите каждую панель для клеток игрового поля и, аналогично тому как делали выше, создайте методы-обработчики для событий Click, MouseEnter и MouseLeave.

Напишем два новых метода CellMouseOver(object sender) и CellMouseOut(object sender) - они и будут выполнять основную работу по смене фона для каждой клетки поля, и их мы будем вызывать из методов-обработчиков для событий MouseEnter и MouseLeave для каждой клетки:
private void CellMouseOver(object sender) {
            if (sender is Panel) {
                Panel panelCell = (Panel)sender;
                panelCell.BackColor = Color.Purple;
                Cursor = Cursors.Hand;
            }
        }

        private void CellMouseOut(object sender) {
            if (sender is Panel) {
                Panel panelCell = (Panel)sender;
                panelCell.BackColor = Color.Indigo;
                Cursor = Cursors.Default;
            }
        }

        private void panelCell0_0_MouseEnter(object sender, EventArgs e) {
            CellMouseOver(sender);
        }

        private void panelCell0_0_MouseLeave(object sender, EventArgs e) {
            CellMouseOut(sender);
        }

        private void panelCell0_1_MouseEnter(object sender, EventArgs e) {
            CellMouseOver(sender);
        }

        private void panelCell0_1_MouseLeave(object sender, EventArgs e) {
            CellMouseOut(sender);
        }

        private void panelCell0_2_MouseEnter(object sender, EventArgs e) {
            CellMouseOver(sender);
        }

        private void panelCell0_2_MouseLeave(object sender, EventArgs e) {
            CellMouseOut(sender);
        }

        private void panelCell1_0_MouseEnter(object sender, EventArgs e) {
            CellMouseOver(sender);
        }

        private void panelCell1_0_MouseLeave(object sender, EventArgs e) {
            CellMouseOut(sender);
        }

        private void panelCell1_1_MouseEnter(object sender, EventArgs e) {
            CellMouseOver(sender);
        }

        private void panelCell1_1_MouseLeave(object sender, EventArgs e) {
            CellMouseOut(sender);
        }

        private void panelCell1_2_MouseEnter(object sender, EventArgs e) {
            CellMouseOver(sender);
        }

        private void panelCell1_2_MouseLeave(object sender, EventArgs e) {
            CellMouseOut(sender);
        }

        private void panelCell2_0_MouseEnter(object sender, EventArgs e) {
            CellMouseOver(sender);
        }

        private void panelCell2_0_MouseLeave(object sender, EventArgs e) {
            CellMouseOut(sender);
        }

        private void panelCell2_1_MouseEnter(object sender, EventArgs e) {
            CellMouseOver(sender);
        }

        private void panelCell2_1_MouseLeave(object sender, EventArgs e) {
            CellMouseOut(sender);
        }

        private void panelCell2_2_MouseEnter(object sender, EventArgs e) {
            CellMouseOver(sender);
        }

        private void panelCell2_2_MouseLeave(object sender, EventArgs e) {
            CellMouseOut(sender);
        }
3.
Попробуйте снова запустить игру и протестируйте то, как теперь меняется курсор и фон при наведении курсора мыши на клетки игрового поля.

Далее мы создадим пустой метод FillCell(Panel panel, int row, int column), который будет в дальнейшем заполнять клетку игрового поля либо "крестиком", либо "ноликом", а также вызовем этот метод из каждого метода-обработчика события Click для всех клеток поля. При этом из каждого обработчика мы передаем координаты клетки - т.е. индекс ряда и столбца (вспомним о том, что наше игровое поле есть матрица размерностью 3x3 элемента):

        private void FillCell(Panel panel, int row, int column) {
                // код метода мы напишем позже, пока здесь будет пусто.
        }

        private void panelCell0_0_Click(object sender, EventArgs e) {
            FillCell((Panel)sender, 0, 0);
        }

        private void panelCell0_1_Click(object sender, EventArgs e) {
            FillCell((Panel)sender, 0, 1);
        }

        private void panelCell0_2_Click(object sender, EventArgs e) {
            FillCell((Panel)sender, 0, 2);
        }

        private void panelCell1_0_Click(object sender, EventArgs e) {
            FillCell((Panel)sender, 1, 0);
        }

        private void panelCell1_1_Click(object sender, EventArgs e) {
            FillCell((Panel)sender, 1, 1);
        }

        private void panelCell1_2_Click(object sender, EventArgs e) {
            FillCell((Panel)sender, 1, 2);
        }

        private void panelCell2_0_Click(object sender, EventArgs e) {
            FillCell((Panel)sender, 2, 0);
        }

        private void panelCell2_1_Click(object sender, EventArgs e) {
            FillCell((Panel)sender, 2, 1);
        }

        private void panelCell2_2_Click(object sender, EventArgs e) {
            FillCell((Panel)sender, 2, 2);
        }

4
К содержимому метода FillCell мы вернёмся чуть позже, когда у нас будет готов класс нашего игрового движка.

Программируем визуальные эффекты для кнопок "Игрок против компьютера" и "Игрок против игрока"
Теперь сделаем так, чтобы применялись эффекты при наведении курсора мыши на кнопки "Игрок против компьютера" и "Игрок против игрока", а также при выведении курсора за области этих кнопок.

Для этого выбираем панель panelPlayerVsCpu и создаем для неё методы-обработчики для событий Click, MouseEnter, MouseLeave.

Для метки labelPlayerVsCpu создаём методы-обработчики для событий Click и MouseEnter.

Аналогично поступаем с панелью panelPlayerVsPlayer и соответствующей ей меткой labelPlayerVsPlayer - для панели создаём обработчики для событий Click, MouseEnter, MouseLeave, а для метки - обработчики для событий Click и MouseEnter.

Поскольку мы хотим, чтобы кнопки у нас меняли стиль единообразно, мы создадим два общих метода для смены стиля кнопок:

RegularButtonMouseOver(Panel panelButton, Label labelButtonText) - будет вызываться при наведении курсора мыши на кнопку. В параметрах метода - панель и метка, для которой надо поменять стиль.
RegularButtonMouseOut(Panel panelButton, Label labelButtonText) - будет вызываться при выводе курсора мыши за область кнопки. В параметрах метода - также панель и метка, для которой надо поменять стиль.
Ниже код для этих методов и их вызов из методов-обработчиков для панелей и меток, отвечающих за кнопки:

        private void RegularButtonMouseOver(Panel panelButton, Label labelButtonText) {
            panelButton.BackColor = Color.Purple;
            labelButtonText.ForeColor = Color.Yellow;
            Cursor = Cursors.Hand;
        }

        private void RegularButtonMouseOut(Panel panelButton, Label labelButtonText) {
            panelButton.BackColor = Color.SlateBlue;
            labelButtonText.ForeColor = Color.Cyan;
            Cursor = Cursors.Default;
        }

        private void panelPlayerVsCpu_MouseEnter(object sender, EventArgs e) {
            RegularButtonMouseOver(panelPlayerVsCpu, labelPlayerVsCpu);
        }

        private void panelPlayerVsCpu_MouseLeave(object sender, EventArgs e) {
            RegularButtonMouseOut(panelPlayerVsCpu, labelPlayerVsCpu);
        }

        private void labelPlayerVsCpu_MouseEnter(object sender, EventArgs e) {
            RegularButtonMouseOver(panelPlayerVsCpu, labelPlayerVsCpu);
        }

        private void panelPlayerVsPlayer_MouseEnter(object sender, EventArgs e) {
            RegularButtonMouseOver(panelPlayerVsPlayer, labelPlayerVsPlayer);
        }

        private void panelPlayerVsPlayer_MouseLeave(object sender, EventArgs e) {
            RegularButtonMouseOut(panelPlayerVsPlayer, labelPlayerVsPlayer);
        }

        private void labelPlayerVsPlayer_MouseEnter(object sender, EventArgs e) {
            RegularButtonMouseOver(panelPlayerVsPlayer, labelPlayerVsPlayer);
        }
Обработчики для событий Click мы пока оставим пустыми - как для панелей, так и для меток с названиями кнопок, вернёмся к ним чуть позже.

Программируем визуальные эффекты для кнопок "Новая игра" и "Сброс"
Кнопки "Новая игра" и "Сброс" будут в том же стиле, как и "Игрок против компьютера" и "Игрок против игрока", поэтому для их стилизации мы используем уже написанные методы RegularButtonMouseOver, RegularButtonMouseOut. 

Выбираем панель с именем panelReset и создаём для неё обработчики событий Click, MouseEnter, MouseLeave.

Выбираем метку с именем labelReset и создаём для неё обработчики событий Click и MouseEnter.

То же самое проделываем для панели panelNewGame и метки labelNewGame.

Теперь для стилизации этих двух кнопок осталось вызывать нужные методы:

        private void panelReset_MouseEnter(object sender, EventArgs e) {
            RegularButtonMouseOver(panelReset, labelReset);
        }

        private void panelReset_MouseLeave(object sender, EventArgs e) {
            RegularButtonMouseOut(panelReset, labelReset);
        }

        private void labelReset_MouseEnter(object sender, EventArgs e) {
            RegularButtonMouseOver(panelReset, labelReset);
        }

        private void panelNewGame_MouseEnter(object sender, EventArgs e) {
            RegularButtonMouseOver(panelNewGame, labelNewGame);
        }

        private void panelNewGame_MouseLeave(object sender, EventArgs e) {
            RegularButtonMouseOut(panelNewGame, labelNewGame);            
        }

        private void labelNewGame_MouseEnter(object sender, EventArgs e) {
            RegularButtonMouseOver(panelNewGame, labelNewGame);
        }
Также пока оставим методы-обработчики события Click для этих панелей и меток пустыми.

Программируем видимость и расположение визуальных элементов
Теперь, прежде чем мы перейдем к написанию самого игрового движка, нам осталось настроить видимость всех наших элементов управления игровым процессом, а также их расположение на главной форме.

Давайте в код формы добавим следующий метод SetPlayersLabelsAndScoreVisible(bool visible), который будет отвечать за отображение/скрытие имён игроков, метки с указанием хода, а также кнопок "Сброс" и "Новая игра":

        private void SetPlayersLabelsAndScoreVisible(bool visible) {
            labelPlayer1Name.Visible = visible;
            labelPlayer1Score.Visible = visible;
            labelPlayer2Name.Visible = visible;
            labelPlayer2Score.Visible = visible;
            labelNowTurnIs.Visible = visible;
            labelWhooseTurn.Visible = visible;

            panelNewGame.Visible = visible;
            panelReset.Visible = visible;
        }
Как видите, метод довольно прост, он принимает единственный параметр visible, которым мы задаём свойства Visible для всех нужных меток и панелей-кнопок.

Теперь выберем главную форму FrmTicTacToe в конструкторе и двойным кликом по ней перейдем в созданный метод-обработчик события Load, т.е. первичной загрузки формы. Разместим там следующий код:

        private void FrmTicTacToe_Load(object sender, EventArgs e) {
            labelPlayer1Name.Text = "?";
            labelPlayer2Name.Text = "?";
            SetPlayersLabelsAndScoreVisible(false);
        }
Здесь изначальные названия игроков будут равны некоторому неопределённому значению "?", а также мы вызываем метод SetPlayersLabelsAndScoreVisible со значением аргумента false, что говорит о том, что при загрузке главной формы лишние элементы управления будут изначально скрыты.

Напишем в коде главной формы ещё один метод ShowMainMenu(bool show):

        private void ShowMainMenu(bool show) {
            labelNewGameTitle.Visible = show;
            panelPlayerVsCpu.Visible = show;
            panelPlayerVsPlayer.Visible = show;
        }
Этот метод будет скрывать или отображать кнопки "Игрок против компьютера" и "Игрок против игрока", а также метку "Новая игра:". 

Добавим также в код главной формы следующий метод UpdateControls():

        private void UpdateControls() {
            ShowMainMenu(false);

            labelPlayer1Name.Text = "Игрок 1"; // engine.GetCurrentPlayer1Title();
            labelPlayer2Name.Text = "Игрок 2"; // engine.GetCurrentPlayer2Title();
            labelWhooseTurn.Text = "Ход игрока N"; // engine.GetWhooseTurnTitle();

            labelPlayer1Name.Top = labelNewGameTitle.Top;
            labelPlayer1Score.Top = labelPlayer1Name.Top;
            labelPlayer2Name.Top = labelPlayer1Name.Top + 37;
            labelPlayer2Score.Top = labelPlayer2Name.Top;
            labelNowTurnIs.Top = labelPlayer2Name.Top + 37;
            labelWhooseTurn.Top = labelNowTurnIs.Top;

            panelNewGame.Left = labelNowTurnIs.Left + 30;
            panelNewGame.Top = labelNowTurnIs.Bottom + 15;

            panelReset.Left = panelNewGame.Right + 15;
            panelReset.Top = panelNewGame.Top;

            SetPlayersLabelsAndScoreVisible(true);
        }
Метод устанавливает местоположение меток с именами игроков, счётом каждого игрока, индикатора хода и кнопок "Новая игра" и "Сброс" на форме, а также вызывает методы ShowMainMenu и SetPlayersLabelsAndScoreVisible. Также обратите внимание, что мы пока устанавливаем имена игроков в захардкоженные строки "Игрок 1", "Игрок 2", а индикатор хода в "Ход игрока N". В этих же строках кода идут комментарии с вызовом методов игрового движка, который позже будет определять корректные названия для текущих игроков и верное значение индикатора хода.

Создаём игровой движок









































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































