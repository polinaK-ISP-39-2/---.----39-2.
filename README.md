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

# UI. Программирование интерфейса и событий на главной форме
Создадим основное поведение для элементов управления на нашей форме. Например, добавим функциональность, которая изменяет цвет фона кнопок при наведении курсора. Когда курсор покинет кнопку, цвет вернётся к исходному.

Начнем с кнопки выхода из игры. Это важно, поскольку она нам понадобится для тестирования интерфейса и поведения нашей формы, что позволит легко завершать программу.

Сначала выберем панель с именем `panelCloseButton` и в окне "Свойства" обратим внимание на значок молнии — это раздел "События" для данной панели. Теперь необходимо двойным кликом по событиям `Click`, `MouseEnter`, `MouseLeave` создать соответствующие методы-обработчики.

Теперь переходим к метке с именем `labelClose`. Повторите тот же процесс и создайте методы-обработчики для событий `Click` и `MouseEnter`.

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
Настало время для создания нашего игрового движка, который будет содержать основную логику, обеспечивающую игровой процесс.

Для этого добавим к нашему проекту 2 новых класса и назовём их Cell и GameEngine. Добавить класс можно, кликнув правой кнопкой мыши на проекте TicTacToe и выбрав в контекстном меню  пункт Добавить → Класс..., как показано ниже:

![csharp-tic-tac-toe-8](https://github.com/user-attachments/assets/24bca255-41f3-45a1-bfc9-6ec0ee9a63a8)

Класс Cell для клетки игрового поля
Класс Cell будет хранить информацию о координатах клетки игрового поля и предоставит нам несколько полезных методов. Ниже показан его код (можно избавиться от всех импортов, добавляемых в класс при его создании - они нам будут не нужны):

namespace TicTacToe {
    internal class Cell {
        public int Row { get; set; }
        public int Column { get; set; }

        private Cell(int row, int column) {
            Row = row;
            Column = column;
        }

        public static Cell From(int row, int column) {
            return new Cell(row, column);
        }

        public static Cell ErrorCell() {
            return new Cell(-1, -1);
        }

        public bool IsErrorCell() {
            return Row == -1 && Column == -1;
        }

        public bool IsValidGameFieldCell() {
            return Row >= 0 && Row <= 2 && Column >= 0 && Column <= 2;
        }
    }
}
Как видим, у класса всего два целочисленных поля Row и Column, которые хранят индекс ряда и столбца в массиве игровых клеток (индексы начинаются с 0). У него есть параметризованный приватный конструктор Cell(int row, int column), который создаёт объект класса с заданными индексами ряда и столбца.

Статический метод From(int row, int column) будет позволять быстро создать объект класса по заданным индексам.

Статический метод ErrorCell() будет возвращать специальную, "ошибочную" клетку, которую мы сможем использовать в возвращаемых значениях методов игрового движка. Признак возврата "ошибочной" клетки будет нам говорить о том, что нужная/искомая клетка на игровом поле не была найдена и требуется сделать какое-то действие.

Метод IsErrorCell() проверяет текущие индексы ряда и столбца для объекта "клетка" и возвращает true, только если оба индекса равны -1.

Метод IsValidGameFieldCell() проверяет, являются ли текущие значения индексов для ряда и столбца допустимыми. Если да, то метод будет возвращать true, иначе false.

Теперь перейдем к написанию класса GameEngine самого игрового движка.

Класс GameEngine для игрового движка
В коде класса GameEngine мы сразу оставим импорт пространства имён System и добавим необходимый импорт для пространства имён System.Drawing, которое нам пригодится в дальнейшем для работы с системными цветами. Остальные неиспользуемые импорты можно удалить.

Добавим в класс два перечисления (enum) с именами GameMode и WhooseTurn:

using System;
using System.Drawing;

namespace TicTacToe {
    internal class GameEngine {
        internal enum GameMode {
            None,
            PlayerVsPlayer,
            PlayerVsCPU
        }

        internal enum WhooseTurn {
            Player1Human,
            Player2Human,
            Player2CPU
        }
    }
}
Первое перечисление задаёт текущий выбранный режим игры (None - игра не началась, PlayerVsPlayer - играют 2 человека, PlayerVsCPU - играет человек с компьютером), второй будет отвечать за индикатор текущего хода (Player1Human - ходит 1-й игрок человек, Player2Human - ходит 2-й игрок человек, Player2CPU - ходит 2-й игрок компьютер).

Добавим в наш класс GameEngine два поля Mode и Turn и установим первичные значения режима игры и того, чей сейчас ход:

        private GameMode Mode { get; set; } = GameMode.None;        
        private WhooseTurn Turn { get; set; } = WhooseTurn.Player1Human;
Добавим поле, которое будет хранить имя игрока-победителя в игре:

        private string Winner { get; set; } = "";
Добавим поля, которые будут содержать количество побед каждого игрока и количество раз, когда случилась ничья:

        private int player1Score = 0;
        private int player2Score = 0;
        private int numberOfDraws = 0;
Также введём символьные константы для обозначения незанятой клетки игрового поля (EMPTY_CELL), занятой крестиком (X_MARK) и ноликом (O_MARK). Также зададим строковые константы, содержащие имена игроков:

        const char EMPTY_CELL = '-';
        const char X_MARK = 'X';
        const char O_MARK = 'O';

        public const string PLAYER_HUMAN_TITLE = "Игрок";
        public const string PLAYER_CPU_TITLE = "Компьютер";
Теперь зададим двухмерный массив символов с именем gameField, который будет представлять наше игровое поле размером 3x3 клетки и по умолчанию инициализируем каждый элемент признаком незанятной клетки (EMPTY_CELL):

        private char[][] gameField = new char[][] {
            new char[] { EMPTY_CELL, EMPTY_CELL, EMPTY_CELL },
            new char[] { EMPTY_CELL, EMPTY_CELL, EMPTY_CELL },
            new char[] { EMPTY_CELL, EMPTY_CELL, EMPTY_CELL }
        };
Создадим несколько полезных методов в классе, которые впоследствии будем использовать:

        public GameMode GetCurrentMode() {
            return Mode;
        }

        public bool IsGameStarted() {
            return Mode != GameMode.None;
        }

        public WhooseTurn GetCurrentTurn() {
            return Turn;
        }

        public string GetWinner() {
            return Winner;
        }

        public bool IsPlayer1HumanTurn() {
            return Turn == WhooseTurn.Player1Human;
        }

        public void SetPlayer1HumanTurn() {
            Turn = WhooseTurn.Player1Human;
        }

        public void ResetScore() {
            player1Score = 0;
            player2Score = 0;
            numberOfDraws = 0;
        }

        public void PrepareForNewGame() {
            Mode = GameMode.None;
            ResetScore();
        }
Кратко их описание:

GetCurrentMode() - возвращает значение текущего режима игры, хранящейся в поле Mode
IsGameStarted() - возвращает true, если игра началась, т.е. значение Mode не равно GameMode.None
GetCurrentTurn() - возвращает текущее значение хода Turn для одного из игроков
GetWinner() - возвращает строку, содержащую имя игрока-победителя в игре
IsPlayer1HumanTurn() - возвращает true, если сейчас ход 1го игрока-человека
SetPlayer1HumanTurn() - устанавливает значение текущего хода Turn в значение WhooseTurn.Player1Human, т.е. ход передаётся 1-му игроку
ResetScore() - сбрасывает счёт обоих игроков в 0, а также обнуляет счётчик игр, сыгранных вничью
PrepareForNewGame() - сбрасывает все счётчики, а также устанавливает значение режима игры в GameMode.None. Подготавливает движок к новой игре.
Теперь добавим в движок метод с именем StartGame, который будет начинать новую игру в одном из двух возможных режимах:

        public void StartGame(GameMode gameMode) {
            if (gameMode == GameMode.None) {
                return;
            }

            ResetScore();

            Mode = gameMode;
            Turn = WhooseTurn.Player1Human;
        }
Логика метода простая: если параметр gameMode равен GameMode.None, то ничего не делаем и возвращаемся из метода. Если же передан правильный режим игры, то сбрасываем все счётчики с помощью вызова ResetScore(), устанавливаем режим движка Mode в значение, переданное при вызове метода, а далее, в зависимости от режима, в котором началась игра, устанавливаем текущий ход для 1-го игрока (т.к. у нас всегда начинает 1-й игрок, играющий за "крестики", независимо от режима игры).

Добавим в движок 2 метода, которые будут возвращать название 1-го и 2-го игроков - эти методы нам пригодятся для вывода в соответствующие метки на главной форме:

        public string GetCurrentPlayer1Title() {
            switch (Mode) {
                case GameMode.PlayerVsCPU:
                    return PLAYER_HUMAN_TITLE;
                case GameMode.PlayerVsPlayer:
                    return PLAYER_HUMAN_TITLE + " 1";                    
            }
            return "";
        }

        public string GetCurrentPlayer2Title() {
            switch (Mode) {
                case GameMode.PlayerVsCPU:
                    return PLAYER_CPU_TITLE;
                case GameMode.PlayerVsPlayer:
                    return PLAYER_HUMAN_TITLE + " 2";
            }
            return "";
        }
Логика методов также довольно простая - в зависимости от текущего режима игры, мы возвращаем значение ранее определённых в классе движка констант с именами игроков. Только если выбран режим игры "Игрок против игрока", то добавляем к константе PLAYER_HUMAN_TITLE цифру 1 или 2, чтобы различать игроков.

Далее добавим метод GetCurrentMarkLabelText, который будет возвращать текущую метку "X" или "O" в зависимости от того, чей сейчас ход:

        public string GetCurrentMarkLabelText() {
            if (IsPlayer1HumanTurn()) {
                return X_MARK.ToString();                
            } else {
                return O_MARK.ToString();                
            }
        }
Добавим ещё один метод GetCurrentMarkLabelColor, который вернёт системный цвет для крестиков и ноликов. Пусть крестики у нас будут окрашены в золотой цвет (Color.Gold), а нолики - в розовый (Color.Fuchsia):

        public Color GetCurrentMarkLabelColor() {
            if (IsPlayer1HumanTurn()) {
                return Color.Gold;
            } else {
                return Color.Fuchsia;
            }
        }
Добавим геттеры для получения счёта для 1го и 2го игроков:

        public int GetPlayer1Score() {
            return player1Score;
        }

        public int GetPlayer2Score() {
            return player2Score;
        }
Также напишем два метода, которые, в зависимости от текущего режима (Mode) и очередности хода (Turn), вернут строку, содержащую имя игрока. GetWhooseTurnTitle() будет возвращать имя игрока, кто ходит в текущий момент времени. GetWhooseNextTurnTitle() - вернёт имя игрока, чей будет следующий ход:

        /// <summary>
        /// Возвращает строку с именем игрока, чей ход в данный момент
        /// </summary>
        /// <returns>строка с именем игрока</returns>
        public string GetWhooseTurnTitle() {
            switch (Mode) {
                case GameMode.PlayerVsCPU:
                    return Turn == WhooseTurn.Player1Human ? PLAYER_HUMAN_TITLE : PLAYER_CPU_TITLE;
                case GameMode.PlayerVsPlayer:
                    return Turn == WhooseTurn.Player1Human ? PLAYER_HUMAN_TITLE + " 1" : PLAYER_HUMAN_TITLE + " 2";
            }
            return "";
        }

        /// <summary>
        /// Возвращает строку с именем игрока, для которого будет следующий ход
        /// </summary>
        /// <returns>строка с именем игрока</returns>
        public string GetWhooseNextTurnTitle() {
            switch (Mode) {
                case GameMode.PlayerVsCPU:
                    return Turn == WhooseTurn.Player1Human ? PLAYER_CPU_TITLE : PLAYER_HUMAN_TITLE;
                case GameMode.PlayerVsPlayer:
                    return Turn == WhooseTurn.Player1Human ? PLAYER_HUMAN_TITLE + " 2" : PLAYER_HUMAN_TITLE + " 1";
            }
            return "";
        }
Теперь добавим следующие методы в наш игровой движок:

ClearGameField() - для очистки всего игрового поля. Под очисткой мы понимаем пробег по нашей матрице gameField размером 3x3 клетки и заполнение каждой клетки значением пустой клетки (EMPTY_CELL)
MakeTurnAndFillGameFieldCell(int row, int column) - для осуществления хода игроком - т.е. для заполнения клетки игрового поля одним из маркеров - X_MARK для крестиков и O_MARK для ноликов, а также смены значения поля Turn, отвечающего за ход
Ниже представлен код этих методов:

        /// <summary>
        /// Очищает игровое поле, заполняя каждую клетку признаком
        /// пустой клетки (по умолчанию это символ '-')
        /// </summary>
        public void ClearGameField() {
            for (int row = 0; row < 3; row++) {
                for (int col = 0; col < 3; col++) {
                    gameField[row][col] = EMPTY_CELL;                    
                }
            }
        }

        public void MakeTurnAndFillGameFieldCell(int row, int column) {
            if (IsPlayer1HumanTurn()) {
                gameField[row][column] = X_MARK;
                if (Mode == GameMode.PlayerVsCPU) {
                    Turn = WhooseTurn.Player2CPU;                    
                } else if (Mode == GameMode.PlayerVsPlayer) {
                    Turn = WhooseTurn.Player2Human;                    
                }
            } else {
                gameField[row][column] = O_MARK;
                Turn = WhooseTurn.Player1Human;
            }
        }
Теперь давайте разберём то, как будет действовать компьютер в режиме игры "Игрок против компьютера". Т.е. фактически сейчас мы проработаем с вами стратегию хода компьютером и заложим её в наш игровой движок.

Ход компьютера разобьём на три фазы или стратегии, которые будут применяться c каждым ходом компьютера, по очереди:

Стратегия 1 ("атакующая") - если до победы компьютеру остаётся всего лишь один ход - (не важно - по горизонтальным, вертикальным или диагональным клеткам), то нужно непременно ставить "нолик" в ту свободную клетку, которая приведёт компьютер к победе. Тем самым компьютер выиграет человека. Это стратегия с первым приоритетом.
Стратегия 2 ("защитная") - если же стратегия 1 не сработала, и на игровом поле нет таких клеток, которые сразу приведут компьютер к победе, то компьютеру нужно применить "защитную" стратегию. Это означает, что ему нужно проверить - "а не выиграет ли меня человек своим следующим ходом?". Т.е. компьютер должен помешать игроку-человеку победить, ставя "нолик" в клетки, ведущие игрока-человека к победе. У этой стратегии второй приоритет выполнения.
Стратегия 3 ("произвольный ход") - применяется, когда неуспешны стратегии 1 и 2. Обычно это характерно для самых первых ходов игры, когда ни один из игроков не имеет выигрышную позицию, ведущую следующим ходом к победе. Мы здесь не будем усложнять и применим рандомизацию - т.е. компьютер будет вычислять произвольную свободную клетку поля и ставить туда "нолик". Это стратегия с наименьшим приоритетом.
Вроде всё понятно, осталось реализовать все три стратегии в нашем движке. Но сперва обратим внимание на некоторую общность между 1-й и 2-й стратегиями - фактически они обе проверяют позиции заполненных клеток - либо на предмет "атаки", либо на предмет "защиты". Разница лишь в том, клетки с какими фигурами ("крестиками" или "ноликами") нужно проверять на предмет того, что по горизонтали, вертикали или диагонали кому-то из игроков остался всего лишь 1 ход до победы.

Поэтому сейчас мы напишем универсальные методы, которые затем будем использовать как в атакующей, так и в защитной стратегиях хода компьютера:

Cell GetHorizontalCellForAttackOrDefence(char checkMark) - метод будет находить клетку по горизонтали, которую нужно "атаковать" для победы или в которую нужно ставить "нолик" при защитной стратегии. Алгоритм метода: бежим двумя циклами по всем клеткам игрового поля - по рядам, сверху-вниз. Для очередного ряда во внутреннем цикле получаем сумму заполненных клеток с определённой фигурой (признак фигуры мы получаем во входном параметре checkMark). Перед внутренним циклом мы задаем переменную int freeCol = -1, которая будет хранить индекс столбца со свободной клеткой в текущем ряду. По выходу из внутреннего цикла по столбцам мы проверяем два признака: если сумма фигур checkMark в текущем ряду равна 2, и при этом индекс столбца со свободной клеткой не равен -1, то это значит, что стратегия сработала, и нам нужно вернуть клетку с индексами текущего ряда и столбца, равного freeCol.
Cell GetVerticalCellForAttackOrDefence(check checkMark) - метод аналогичен предыдущему, только пробег по игровому полю уже осуществляем по столбцам, сверху-вниз, поэтому внешний цикл организован по столбцам, а внутренний - по рядам. Вместо freeCol теперь у нас переменная freeRow, и в неё мы пытаемся сохранить индекс ряда со свободной клеткой (если такая есть). Принцип действия такой же, только теперь мы считаем сумму фигур checkMark по столбцам, и в случае срабатывания стратегии вернём клетку с индексом ряда freeRow и текущего столбца, в котором находимся.
Cell GetDiagonalCellForAttackOrDefence(check checkMark) - метод похож на два предыдущих, но с некоторыми отличиями: во-первых, у нас теперь только один цикл по рядам, а индекс столбца, который выведет нас на клетку, принадлежащую одной или второй диагонали, мы вычисляем по формуле. Под первой диагональю будем понимать ту, что проходит через клетки с индексами (0; 0), (1; 1), (2; 2). Легко догадаться, что формула вычисления индекса столбца по индексу ряда следующая: <column> = row, где row - текущий ряд, а <column> - вычисляемый индекс столбца. Вторая же диагональ проходит через клетки с индексами (0; 2), (1; 1), (2; 0). И в этом случае индекс столбца для клетки, принадлежащей диагонали, вычисляется так: <column> = 2 - row, где row - текущий ряд, а <column> - вычисляемый индекс столбца. Мы также должны хранить две отдельных суммы для каждой диагонали, в которые будем добавлять 1, если текущая просматриваемая клетка равна интересующей нас фигуре checkMark. И отдельно храним пары индексов freeRow1, freeCol1 и freeRow2, freeCol2 для свободной клетки по первой и второй диагонали. Как и в двух других методах, мы проверяем, что если сумма интересующих фигур в диагонали равна 2, и при этом на диагонали есть свободная клетка, то мы вернём её координаты.
Ниже представлен код этих трёх методов:

        private Cell GetHorizontalCellForAttackOrDefence(char checkMark) {
            for (int row = 0; row < 3; row++) {
                int currentSumHorizontal = 0;
                int freeCol = -1;
                for (int col = 0; col < 3; col++) {
                    if (gameField[row][col] == EMPTY_CELL) {
                        freeCol = col;
                    }
                    currentSumHorizontal += gameField[row][col] == checkMark ? 1 : 0;
                }

                if (currentSumHorizontal == 2 && freeCol >= 0) {
                    return Cell.From(row, freeCol);
                }
            }
            return Cell.ErrorCell();
        }

        private Cell GetVerticalCellForAttackOrDefence(char checkMark) {            
            for (int col = 0; col < 3; col++) {
                int currentSumVert = 0;
                int freeRow = -1;
                for (int row = 0; row < 3; row++) {
                    if (gameField[row][col] == EMPTY_CELL) {
                        freeRow = row;
                    }
                    currentSumVert += gameField[row][col] == checkMark ? 1 : 0;
                }

                if (currentSumVert == 2 && freeRow >= 0) {
                    return Cell.From(freeRow, col);
                }
            }
            return Cell.ErrorCell();
        }

        private Cell GetDiagonalCellForAttackOrDefence(char checkMark) {
            // диагональ 1:
            // * - -
            // - * -
            // - - *
            // координаты клеток: (0; 0), (1; 1), (2; 2)
            // формула для вычисления столбца по ряду: <column> = row

            // диагональ 2:
            // - - *
            // - * -
            // * - -
            // координаты клеток: (0; 2), (1; 1), (2, 0)
            // формула для вычисления столбца по ряду: <column> = 2 - row

            int diagonal1Sum = 0;
            int diagonal2Sum = 0;
            int freeCol1 = -1, freeRow1 = -1;
            int freeCol2 = -1, freeRow2 = -1;
            for (int row = 0; row < 3; row++) {
                diagonal1Sum += gameField[row][row] == checkMark ? 1 : 0;
                diagonal2Sum += gameField[row][2 - row] == checkMark ? 1 : 0;

                if (gameField[row][row] == EMPTY_CELL) {
                    freeCol1 = row;
                    freeRow1 = row;
                }
                if (gameField[row][2 - row] == EMPTY_CELL) {
                    freeCol2 = 2 - row;
                    freeRow2 = row;
                }

                if (diagonal1Sum == 2 && freeRow1 >= 0 && freeCol1 >= 0) {
                    return Cell.From(freeRow1, freeCol1);
                } else if (diagonal2Sum == 2 && freeRow2 >= 0 && freeCol2 >= 0) {
                    return Cell.From(freeRow2, freeCol2);
                }
            }

            return Cell.ErrorCell();
        }
Теперь, когда универсальные методы для "атакующей" и "защитной" стратегий для компьютера готовы, нам остаётся их использовать.

Напишем в классе движка следующий код:

        private Cell ComputerTryAttackHorizontalCell() {
            return GetHorizontalCellForAttackOrDefence(O_MARK);
        }

        private Cell ComputerTryAttackVerticalCell() {
            return GetVerticalCellForAttackOrDefence(O_MARK);
        }

        private Cell ComputerTryAttackDiagonalCell() {
            return GetDiagonalCellForAttackOrDefence(O_MARK);
        }

        private Cell ComputerTryDefendHorizontalCell() {
            return GetHorizontalCellForAttackOrDefence(X_MARK);
        }

        private Cell ComputerTryDefendVerticalCell() {
            return GetVerticalCellForAttackOrDefence(X_MARK);
        }

        private Cell ComputerTryDefendDiagonalCell() {
            return GetDiagonalCellForAttackOrDefence(X_MARK);
        }

        private Cell ComputerTryAttackCell() {
            // Пытаемся атаковать по горизонтальным клеткам
            Cell attackedHorizontalCell = ComputerTryAttackHorizontalCell();
            if (!attackedHorizontalCell.IsErrorCell()) {
                return attackedHorizontalCell;
            }

            // Пытаемся атаковать по вертикальным клеткам
            Cell attackedVerticalCell = ComputerTryAttackVerticalCell();
            if (!attackedVerticalCell.IsErrorCell()) {
                return attackedVerticalCell;
            }

            // Пытаемся атаковать по диагональным клеткам
            Cell attackedDiagonalCell = ComputerTryAttackDiagonalCell();
            if (!attackedDiagonalCell.IsErrorCell()) {
                return attackedDiagonalCell;
            }

            // Нет приемлемых клеток для атаки - возвращаем спецклетку с признаком ошибки
            return Cell.ErrorCell();
        }

        private Cell ComputerTryDefendCell() {
            // Пытаемся защищаться по горизонтальным клеткам
            Cell defendedHorizontalCell = ComputerTryDefendHorizontalCell();
            if (!defendedHorizontalCell.IsErrorCell()) {
                return defendedHorizontalCell;
            }

            // Пытаемся защищаться по вертикальным клеткам
            Cell defendedVerticalCell = ComputerTryDefendVerticalCell();
            if (!defendedVerticalCell.IsErrorCell()) {
                return defendedVerticalCell;
            }

            // Пытаемся защищаться по диагональным клеткам
            Cell defendedDiagonalCell = ComputerTryDefendDiagonalCell();
            if (!defendedDiagonalCell.IsErrorCell()) {
                return defendedDiagonalCell;
            }

            // Нет приемлемых клеток для обороны - возвращаем спецклетку с признаком ошибки
            return Cell.ErrorCell();
        }
Как видите, здесь два ключевых метода для осуществления компьютерного хода:

ComputerTryAttackCell() - пытается "атаковать" свободную клетку для выигрыша компьютера, по очереди проверяя клетки по горизонтали, вертикали и диагонали. Если клетки для атаки не найдены, возвращается спецклетка с индексами (-1; -1), что означает, что атакующая стратегия не сработала.
ComputerTryDefendCell() - пытается "защитить" клетку от ближайшего выигрыша игроком-человеком, по очереди проверяя клетки по горизонтали, вертикали и диагонали. Если клетки для защиты не найдены, возвращается спецклетка с индексами (-1; -1), что означает, что защитная стратегия не сработала.
Теперь, когда мы запрограммировали "атакующую" и "защитную" стратегии компьютера, осталась последняя стратегия - выбор произвольной клетки.

Реализуем её в методе ComputerTrySelectRandomFreeCell():

        private Cell ComputerTrySelectRandomFreeCell() {
            Random random = new Random();
            int randomRow, randomCol;
            const int max_attempts = 1000;  // кол-во попыток можно настроить по вкусу. чем больше, тем вероятнее может произойти подвисание, когда 
                                            // свободных клеток на поле всё меньше, и рандомный перебор и поиск свободной не приносит быстрого результата
            int current_attempt = 0;
            do {
                randomRow = random.Next(3);
                randomCol = random.Next(3);
                current_attempt++;
            } while (gameField[randomRow][randomCol] != EMPTY_CELL && current_attempt <= max_attempts);

            if (current_attempt > max_attempts) {
                // мы не смогли выбрать рандомную свободную клетку за 1000 попыток, поэтому выбираем вручную
                // ближайшую клетку простым перебором по всем клеткам игрового поля
                for (int row = 0; row < 3; row++) {
                    for (int col = 0; col < 3; col++) {
                        if (gameField[row][col] == EMPTY_CELL) {
                            // клетка свободна, сразу возвращаем её
                            return Cell.From(row, col);
                        }
                    }
                }
            }

            return Cell.From(randomRow, randomCol);
        }
Этот метод генерирует произвольный индекс ряда (randomRow) и столбца (randomRow) в цикле - до тех пор, пока сгенерированные индексы не дают нам свободную от "крестиков" и "ноликов" клетку. Во избежание ситуации, когда мы так и не сможем рандомно найти свободную клетку мы предусматриваем максимальное количество попыток поиска свободной клетки, которое задаём в константе max_attempts. Каждую итерацию цикла мы увеличиваем "счётчик попыток подбора свободной клетки", хранимый в переменной current_attempt.

По выходу из цикла мы проверим - исчерпался ли счётчик попыток подбора. Если да, мы задействуем самую элементарную стратегию подбора свободной для хода клетки - просто бежим по рядам и столбцам по игровому полю до тех пор, пока не найдем свободную клетку (EMPTY_CELL) и её же вернем из метода.

Но, если сработала логика произвольного подбора свободной клетки, то последней строкой метода мы вернём найденную клетку с индексами (randomRow, randomCol).

Теперь давайте добавим следующие два метода в движок - IsAnyFreeCell(), который вернёт true, если на поле осталась хоть одна свободная клетка, а также метод MakeComputerTurnAndGetCell(), который и делает "ход компьютера" - т.е. применяет все три стратегии по очереди и возвращает координаты клетки, выбранной компьютером:

        /// <summary>
        /// Возвращает true, если есть хотя бы одна незанятая клетка на игровом поле и false в противном случае
        /// </summary>
        /// <returns>true при наличии хотя бы одной свободной клетки на поле, иначе false</returns>
        public bool IsAnyFreeCell() {            
            for (int row = 0; row < 3; row++) {
                for (int col = 0; col < 3; col++) {
                    if (gameField[row][col] == EMPTY_CELL) {
                        return true;
                    }
                }
            }
            return false;
        }

        public Cell MakeComputerTurnAndGetCell() {
            // Стратегия 1 - компьютер пытается сначала атаковать, если ему до победы остался всего лишь один ход
            Cell attackedCell = ComputerTryAttackCell();
            if (!attackedCell.IsErrorCell()) {
                return attackedCell;
            }

            // Стратегия 2 - если нет приемлемых клеток для атаки, компьютер попытается найти клетки, которые нужно защитить,
            // чтобы предотвратить победу человека
            Cell defendedCell = ComputerTryDefendCell();
            if (!defendedCell.IsErrorCell()) {
                return defendedCell;
            }

            // Стратегия 3 - у комьютера нет приемлемых клеток для атаки и защиты, поэтому ему нужно выбрать произвольную свободную клетку
            // для его очередного хода
            if (IsAnyFreeCell()) {
                Cell randomFreeCell = ComputerTrySelectRandomFreeCell();
                return randomFreeCell;
            }

            return Cell.ErrorCell();
        }
Друзья, позади уже много кода, и нам осталась самая малость - для завершения написания движка нам нужно реализовать в нём две последние вещи:

логику определения, что произошла ничья - мы должны проверить, что на поле не осталось больше свободных клеток и, если это так, то увеличить счётчик для игр вничью
логику определения, что произошла победа - для этого мы должны подсчитать сумму одинаковых фигур по горизонтали, вертикали и диагоналям. Если эта сумма равна 3, то в зависимости от фигуры провозгласить, какой игрок победил и увеличить переменную его счёта.
Давайте добавим эту логику в игровой движок:

Метод IsDraw() - вернёт true, если произошла ничья, а также увеличит счётчик игр вничью:

        /// <summary>
        /// Возвращает true и увеличивает счётчик ничьих, если произошла очередная ничья.        
        /// </summary>
        /// <returns>true, если произошла ничья, в противном случае false</returns>
        public bool IsDraw() {
            bool isNoFreeCellsLeft = !IsAnyFreeCell();
            if (isNoFreeCellsLeft) {                
                numberOfDraws++;
            }
            return isNoFreeCellsLeft;
        }
Три метода, которые проверяют признак победы одного из игроков:

CheckWinOnHorizontalCellsAndUpdateWinner() - проверит победителя по горизонтальным клеткам и обновит поле Winner, а также увеличит соответствующий счётчик побед
CheckWinOnVerticalCellsAndUpdateWinner() - - проверит победителя по вертикальным клеткам и обновит поле Winner, а также увеличит соответствующий счётчик побед
CheckWinOnDiagonalCellsAndUpdateWinner() - проверит победителя по диагональным клеткам и обновит поле Winner, а также увеличит соответствующий счётчик побед
        /// <summary>
        /// Проверяет наличие победы какого-либо из игроков по горизонтальным клеткам игрового поля
        /// </summary>
        /// <returns></returns>
        private bool CheckWinOnHorizontalCellsAndUpdateWinner() {
            for (int row = 0; row < 3; row++) {
                int sumX = 0; int sumO = 0;
                for (int col = 0; col < 3; col++) {
                    sumX += gameField[row][col] == X_MARK ? 1 : 0;
                    sumO += gameField[row][col] == O_MARK ? 1 : 0;
                }
                if (sumX == 3) {
                    // X победили
                    Winner = Mode == GameMode.PlayerVsPlayer ? PLAYER_HUMAN_TITLE + " 1" : PLAYER_HUMAN_TITLE;
                    player1Score++;
                    return true;
                } else if (sumO == 3) {
                    // O победили
                    Winner = Mode == GameMode.PlayerVsPlayer ? PLAYER_HUMAN_TITLE + " 2" : PLAYER_CPU_TITLE;
                    player2Score++;
                    return true;
                }
            }
            return false;
        }

        /// <summary>
        /// Проверяет наличие победы какого-либо из игроков по вертикальным клеткам игрового поля
        /// </summary>
        /// <returns></returns>
        private bool CheckWinOnVerticalCellsAndUpdateWinner() {
            for (int col = 0; col < 3; col++) {
                int sumX = 0; int sumO = 0;
                for (int row = 0; row < 3; row++) {
                    sumX += gameField[row][col] == X_MARK ? 1 : 0;
                    sumO += gameField[row][col] == O_MARK ? 1 : 0;
                }

                if (sumX == 3) {
                    // X победили
                    Winner = Mode == GameMode.PlayerVsPlayer ? PLAYER_HUMAN_TITLE + " 1" : PLAYER_HUMAN_TITLE;
                    player1Score++;
                    return true;
                } else if (sumO == 3) {
                    // O победили
                    Winner = Mode == GameMode.PlayerVsPlayer ? PLAYER_HUMAN_TITLE + " 2" : PLAYER_CPU_TITLE;
                    player2Score++;
                    return true;
                }
            }
            return false;
        }

        /// <summary>
        /// Проверяет наличие победы какого-либо из игроков по диагональным клеткам игрового поля
        /// </summary>
        /// <returns></returns>
        private bool CheckWinOnDiagonalCellsAndUpdateWinner() {
            int diag1sumX = 0, diag2sumX = 0;
            int diag1sumO = 0, diag2sumO = 0;
            for (int row = 0; row < 3; row++) {
                if (gameField[row][row] == O_MARK) {
                    diag1sumO++;
                }
                if (gameField[row][row] == X_MARK) {
                    diag1sumX++;
                }
                if (gameField[row][2 - row] == O_MARK) {
                    diag2sumO++;
                }
                if (gameField[row][2 - row] == X_MARK) {
                    diag2sumX++;
                }
            }

            if (diag1sumX == 3 || diag2sumX == 3) {
                Winner = Mode == GameMode.PlayerVsPlayer ? PLAYER_HUMAN_TITLE + " 1" : PLAYER_HUMAN_TITLE;
                player1Score++;
                return true;
            } else if (diag1sumO == 3 || diag2sumO == 3) {
                Winner = Mode == GameMode.PlayerVsPlayer ? PLAYER_HUMAN_TITLE + " 2" : PLAYER_CPU_TITLE;
                player2Score++;
                return true;
            }

            return false;
        }
Наконец, остался последний метод для нашего движка - IsWin(), который по очереди вызывает три предыдущих и возвращает true, если была победа, в противном случае возвращает false:

        /// <summary>
        /// Возвращает true, если кто-то из игроков выиграл
        /// </summary>
        /// <returns>true, если какой-то из игроков выиграл, иначе false</returns>
        public bool IsWin() {
            if (CheckWinOnHorizontalCellsAndUpdateWinner()) {
                return true;
            }

            if (CheckWinOnVerticalCellsAndUpdateWinner()) {
                return true;
            }

            if (CheckWinOnDiagonalCellsAndUpdateWinner()) {
                return true;
            }

            return false;
        }
Самое сложное позади, наш игровой движок полностью готов! Теперь нам осталось лишь подключить и задействовать его в главной форме нашего приложения.

UI + GameEngine. Подключаем готовый игровой движок к главной форме приложения
Возвратимся на главную форму FrmTicTacToe для нашей игры. В самое начало класса добавляем новое поле engine класса GameEngine и инициализируем наш движок с помощью оператора new:

using System;
using System.Drawing;
using System.Windows.Forms;

namespace TicTacToe {
    public partial class FrmTicTacToe : Form {
        // подключаем наш игровой движок:
        private GameEngine engine = new GameEngine();
        
        // ... ранее написанный код формы ...
    }
}
Добавим к главной форме также следующие новые методы:

GetPanelCellControlByCell(Cell cell) - получает по экземпляру клетки cell название элемента Panel, отвечающего за эту клетку игрового поля. Затем среди всех элементов формы находит панель с данным названием и возвращает её в виде экземпляра класса Panel.
ClearGameField() - вызывает очистку игрового поля через одноимённый метод игрового движка, а также очищает всё внутреннее содержимое панели, представляющей клетку
        private Panel GetPanelCellControlByCell(Cell cell) {            
            if (cell == null || !cell.IsValidGameFieldCell()) {
                return null;
            }
            string panelCtrlName = "panelCell" + cell.Row + "_" + cell.Column;
            foreach (Control ctrl in this.Controls) {
                if (ctrl.Name.Equals(panelCtrlName) && ctrl is Panel) {
                    return (Panel)ctrl;
                }
            }

            return null;
        }

        private void ClearGameField() {
            engine.ClearGameField();

            for (int row = 0; row < 3; row++) {
                for (int col = 0; col < 3; col++) {
                    Panel panelCell = GetPanelCellControlByCell(Cell.From(row, col));
                    if (panelCell != null) {
                        panelCell.Controls.Clear();
                    }
                }
            }

            engine.SetPlayer1HumanTurn();
            labelWhooseTurn.Text = engine.GetWhooseTurnTitle();
        }
Помните метод FillCell(Panel panel, int row, int column) на главной форме, который мы добавили, но оставили пустым? Пришло время наполнить его логикой, поскольку наш игровой движок уже готов.

Напишем в нём следующий код:

        private void FillCell(Panel panel, int row, int column) {
            if (!engine.IsGameStarted()) {
                // если игра не началась, не рисовать ничего на игровом поле и просто вернуться
                return; 
            }

            Label markLabel = new Label();
            markLabel.Font = new Font(FontFamily.GenericMonospace, 72, FontStyle.Bold);
            markLabel.AutoSize = true;
            markLabel.Text = engine.GetCurrentMarkLabelText();
            markLabel.ForeColor = engine.GetCurrentMarkLabelColor();

            labelWhooseTurn.Text = engine.GetWhooseNextTurnTitle();

            engine.MakeTurnAndFillGameFieldCell(row, column);

            panel.Controls.Add(markLabel);

            if (engine.IsWin()) {
                // Движок вернул результат, что произошла победа одного из игроков
                MessageBox.Show("Победа! Выиграл " + engine.GetWinner(), "Крестики-Нолики", MessageBoxButtons.OK, MessageBoxIcon.Information);
                labelPlayer1Score.Text = engine.GetPlayer1Score().ToString();
                labelPlayer2Score.Text = engine.GetPlayer2Score().ToString();
                ClearGameField();
            } else if (engine.IsDraw()) {
                // Движок вернул результат, что произошла ничья
                MessageBox.Show("Ничья!", "Крестики-Нолики", MessageBoxButtons.OK, MessageBoxIcon.Information);
                ClearGameField();
            } else {
                // Ещё остались свободные клетки на поле. Если ход компьютера - вызываем движок для определения клетки, которую
                // выберет комьютер для хода
                if (engine.GetCurrentTurn() == GameEngine.WhooseTurn.Player2CPU) {
                    Cell cellChosenByCpu = engine.MakeComputerTurnAndGetCell();
                    if (!cellChosenByCpu.IsErrorCell()) {
                        Panel panelCell = GetPanelCellControlByCell(cellChosenByCpu);
                        if (panelCell != null) {
                            FillCell(panelCell, cellChosenByCpu.Row, cellChosenByCpu.Column);
                        } else {
                            // что-то пошло не так, мы не смогли найти верный элемент Panel по клетке, выбранной компьютером
                            // покажем ошибку
                            MessageBox.Show(
                                "Произошла ошибка: выбранная компьютером клетка не должна быть равна null!",
                                "Крестики-Нолики",
                                MessageBoxButtons.OK,
                                MessageBoxIcon.Error
                            );
                        }
                    } else {
                        // что-то пошло не так, движок вернул спецклетку, хотя такого быть не должно.
                        // покажем ошибку
                        MessageBox.Show(
                            "Произошла ошибка: компьютер не смог выбрать клетку для хода!", 
                            "Крестики-Нолики", 
                            MessageBoxButtons.OK, 
                            MessageBoxIcon.Error
                        );
                    }
                }
            }
        }
Как можно видеть, метод FillCell динамически создаёт новую метку (Label) с определённым шрифтом, текстом и стилем. Это и есть маркер "крестика" или "нолика" для заполнения клетки игрового поля, которой является определённая панель (Panel), передаваемая методу в параметре panel. Координаты же клетки нужны для маркировки клетки игрового поля в самом движке - чтобы пометить эту клетку как занятую.

Метод также проверяет, произошла ли победа кого-то из игроков сразу после заполнения клетки игрового поля. Если да, то выводится сообщение о победе игрока, обновляется текст со счётом игроков и производится очистка игрового поля. Если победы не было, но была ничья, то также выводится сообщение и очищается игровое поле. Наконец, если не было победы и не произошла ничья, то в случае режима игры "Игрок против компьютера" мы вызываем метод движка MakeComputerTurnAndGetCell(), который выполнит расчёт логики компьютера и выберет клетку для хода, которую мы тут же заполним с помощью этого же метода FillCell. Если же играют два человека, то просто они по очереди ставят "крестики" и "нолики" до тех пор, пока кто-то не выиграет, либо не произойдет ничья.

Добавим пару методов: ResetGame() - для сброса игры и StartNewGame() - для начала новой игры:

        private void ResetGame() {
            ClearGameField();
            engine.StartGame(engine.GetCurrentMode());
            labelPlayer1Score.Text = engine.GetPlayer1Score().ToString();
            labelPlayer2Score.Text = engine.GetPlayer2Score().ToString();
            UpdateControls();
        }

        private void StartNewGame() {
            ClearGameField();
            engine.PrepareForNewGame();
            
            labelPlayer1Score.Text = engine.GetPlayer1Score().ToString();
            labelPlayer2Score.Text = engine.GetPlayer2Score().ToString();

            ShowMainMenu(true);
            SetPlayersLabelsAndScoreVisible(false);
        }
Главная форма - последние штрихи
В методе UpdateControls() главной формы мы теперь можем раскомментировать строки, убрав хардкод для имён игроков и индикатора хода, обращаясь к нашему игровому движку:

        private void UpdateControls() {
            // ...
            labelPlayer1Name.Text = engine.GetCurrentPlayer1Title();
            labelPlayer2Name.Text = engine.GetCurrentPlayer2Title();
            labelWhooseTurn.Text = engine.GetWhooseTurnTitle();
            // ...
        }
Заполним пустые обработчики для события Click при нажатии на кнопку "Новая игра":

        private void panelNewGame_Click(object sender, EventArgs e) {
            StartNewGame();
        }

        private void labelNewGame_Click(object sender, EventArgs e) {
            StartNewGame();
        }
Аналогичным образом заполним пустые обработчики для события Click при нажатии на кнопку "Сброс":

        private void panelReset_Click(object sender, EventArgs e) {
            ResetGame();
        }

        private void labelReset_Click(object sender, EventArgs e) {
            ResetGame();
        }
Также добавим простой метод StartNewGameInSelectedMode(GameEngine.GameMode selectedMode) и заполним пустые обработчики для события Click для кнопок "Игрок против компьютера" и "Игрок против игрока":

        private void StartNewGameInSelectedMode(GameEngine.GameMode selectedMode) {
            engine.StartGame(selectedMode);
            UpdateControls();
        }

        private void panelPlayerVsCpu_Click(object sender, EventArgs e) {
            StartNewGameInSelectedMode(GameEngine.GameMode.PlayerVsCPU);
        }

        private void panelPlayerVsPlayer_Click(object sender, EventArgs e) {
            StartNewGameInSelectedMode(GameEngine.GameMode.PlayerVsPlayer);
        }

        private void labelPlayerVsCpu_Click(object sender, EventArgs e) {
            StartNewGameInSelectedMode(GameEngine.GameMode.PlayerVsCPU);
        }

        private void labelPlayerVsPlayer_Click(object sender, EventArgs e) {
            StartNewGameInSelectedMode(GameEngine.GameMode.PlayerVsPlayer);
        }
Ну вот и всё, мы закончили написание игры "Крестики-Нолики" на языке C# с использованием Windows Forms. Можете теперь запустить игру и попробовать её в одном из доступных режимов.

Примерно так выглядит игровой процесс в нашей игре:

![csharp-tic-tac-toe-9](https://github.com/user-attachments/assets/5ec80db3-b48a-4768-9625-0afe87c49bef)




































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































