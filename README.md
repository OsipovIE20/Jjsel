# Jjsel

Полное руководство по созданию проекта с авторизацией
1. Создание репозитория на GitHub
1.1 Регистрация и вход
Перейдите на github.com

Зарегистрируйтесь (если нет аккаунта)

Войдите в свою учетную запись

1.2 Создание нового репозитория
Нажмите "+" в правом верхнем углу → "New repository"

Заполните данные:

Repository name: AuthApp

Description: "Приложение авторизации с переходом на успешную страницу"

Public/Private: выберите нужное

Initialize this repository with: Добавьте .gitignore (Visual Studio) и лицензию (MIT)

Нажмите "Create repository"

2. Настройка проекта в GitHub
2.1 Создание Kanban доски
В репозитории перейдите во вкладку "Projects"

Нажмите "New project"

Выберите "Board" (Kanban)

Назовите "AuthApp Development"

Нажмите "Create"

2.2 Создание Issues
Во вкладке "Issues" создайте:

"Прототипирование интерфейса" (метка design)

"Реализация формы авторизации" (метка feature)

"Создание страницы успешной авторизации" (метка feature)

"Настройка перехода между формами" (метка logic)

"Документация проекта" (метка documentation)

3. Локальная настройка проекта
3.1 Клонирование репозитория
bash
Copy
git clone https://github.com/ваш-логин/AuthApp.git
cd AuthApp
3.2 Создание проекта в Visual Studio
Откройте Visual Studio 2019+

Создайте новый проект:

Тип: Windows Forms App (.NET Framework)

Название: AuthApp

Расположение: выберите папку с клонированным репозиторием

4. Реализация приложения
4.1 Форма авторизации (LoginForm.cs)
csharp
Copy
public partial class LoginForm : Form
{
    public LoginForm()
    {
        InitializeComponent();
        passwordTextBox.UseSystemPasswordChar = true;
    }

    private void loginButton_Click(object sender, EventArgs e)
    {
        if (usernameTextBox.Text == "admin" && passwordTextBox.Text == "12345")
        {
            SuccessForm successForm = new SuccessForm();
            successForm.Show();
            this.Hide();
        }
        else
        {
            MessageBox.Show("Неверные учетные данные", "Ошибка", 
                          MessageBoxButtons.OK, MessageBoxIcon.Error);
        }
    }

    private void togglePasswordButton_Click(object sender, EventArgs e)
    {
        passwordTextBox.UseSystemPasswordChar = !passwordTextBox.UseSystemPasswordChar;
    }
}
4.2 Форма успешной авторизации (SuccessForm.cs)
csharp
Copy
public partial class SuccessForm : Form
{
    public SuccessForm()
    {
        InitializeComponent();
        
        // Анимация появления
        this.Opacity = 0;
        Timer fadeIn = new Timer();
        fadeIn.Interval = 20;
        fadeIn.Tick += (s, args) =>
        {
            if (this.Opacity >= 1) 
                fadeIn.Stop();
            else 
                this.Opacity += 0.05;
        };
        fadeIn.Start();
    }

    private void logoutButton_Click(object sender, EventArgs e)
    {
        LoginForm loginForm = new LoginForm();
        loginForm.Show();
        this.Close();
    }
}
5. Настройка переходов между формами
5.1 Program.cs (точка входа)
csharp
Copy
static class Program
{
    [STAThread]
    static void Main()
    {
        Application.EnableVisualStyles();
        Application.SetCompatibleTextRenderingDefault(false);
        Application.Run(new LoginForm());
    }
}
5.2 Анимация перехода
Добавьте в LoginForm перед переходом:

csharp
Copy
this.Opacity = 1;
Timer fadeOut = new Timer();
fadeOut.Interval = 20;
fadeOut.Tick += (s, args) =>
{
    if (this.Opacity <= 0)
    {
        fadeOut.Stop();
        SuccessForm successForm = new SuccessForm();
        successForm.Show();
        this.Hide();
    }
    else
    {
        this.Opacity -= 0.05;
    }
};
fadeOut.Start();
6. Создание правильного README.md
markdown
Copy
# AuthApp

![Скриншот авторизации](screenshots/login.png)

## Описание
Приложение с безопасной авторизацией и переходом на страницу успеха

## Функционал
- **Авторизация** по логину/паролю
- Анимированный **переход** между формами
- Страница **успешной авторизации**

## Учетные данные
Логин: `admin`  
Пароль: `12345`

## Установка
```bash
git clone https://github.com/ваш-логин/AuthApp.git
Скриншоты
Форма входа	Успешная авторизация
Login	Success
Copy

## 7. Финализация проекта

1. Добавьте все файлы в Git:
```bash
git add .
git commit -m "Реализована авторизация с переходом на успешную страницу"
git push origin main
Проверьте, что все Issues перемещены в "Done" на Kanban доске

Обновите README.md с актуальными скриншотами и описанием

Теперь при вводе правильных учетных данных (admin/12345) пользователь будет перенаправлен на форму успешной авторизации с плавной анимацией перехода.

