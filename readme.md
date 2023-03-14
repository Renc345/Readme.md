# Программа StudentsApp реализует функционал:
* __Описание программы__

 Программа предназначена для добавления студента по групее с заполнением его данных.
* __Функционал программы__

1.Кнопка сохранить(Для сохраннения студентов).

2.Унопка удалить(Для удаления студентов).

3.Кнопка отмена(Для отмены изменений).

4.Кнопка добавить(Для добавления студентов).

5.Поле со списком(Для выбора групп).
* __Подключенная библиотека__

Using SQLite
* __Используемые классы__

1.Класс Student

```
public class Student
    {
        
        
        [PrimaryKey, AutoIncrement, Column("_id")]
        public int Id { get; set; }
         
        public string Group { get; set;}
        public string Name { get; set; }
        public string Email { get; set; }
        public string Phone { get; set; }
        public string Data { get; set; }
    }
```
2.Класс StudentsPage
```
public partial class StudentsPage : ContentPage
{
	public StudentsPage()
	{
		InitializeComponent();
	}

    private void SaveFriend(object sender, EventArgs e)
    {
        var student = (Student)BindingContext;
        if (!String.IsNullOrEmpty(student.Name))
        {
            App.Database.SaveItem(student);
        }
        this.Navigation.PopAsync();
    }
    private void DeleteFriend(object sender, EventArgs e)
    {
        var student = (Student)BindingContext;
        App.Database.DeleteItem(student.Id);
        this.Navigation.PopAsync();
    }
    private void Cancel(object sender, EventArgs e)
    {
        this.Navigation.PopAsync();
    }

    private void GroupPicker_SelectedIndexChanged(object sender, EventArgs e)
    {

    }
}
```
3.Класс StudentRepository

```
public class StudentRepository
    {

        SQLiteConnection database;

        static object locker = new object();
        public StudentRepository(string databasePath)
        {
            database = new SQLiteConnection(databasePath);
            database.CreateTable<Student>();
        }
        public IEnumerable<Student> GetItems()
        {
            return database.Table<Student>().ToList();
        }
        public Student GetItem(int id)
        {
            return database.Get<Student>(id);
        }
        public int DeleteItem(int id)
        {
            lock (locker)
            {
                return database.Delete<Student>(id);
            }
        }
        public int SaveItem(Student item)
        {
            if (item.Id != 0)
            {
                database.Update(item);
                return item.Id;
            }
            else
            {
                return database.Insert(item);
            }
        }
    }
```

4.Класс MauiProgram
```
public static class MauiProgram
{
	public static MauiApp CreateMauiApp()
	{
		var builder = MauiApp.CreateBuilder();
		builder
			.UseMauiApp<App>()
			.ConfigureFonts(fonts =>
			{
				fonts.AddFont("OpenSans-Regular.ttf", "OpenSansRegular");
				fonts.AddFont("OpenSans-Semibold.ttf", "OpenSansSemibold");
			});

		return builder.Build();
	}
}
```
![Лого TexTerra](/image2.png "Наш логотип")
![Лого TexTerra](/image3.png "Наш логотип")