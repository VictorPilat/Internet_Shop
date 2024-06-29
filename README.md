# Інтернет-магазин

## Список учасників команди
>- Бояркіна Орина(TEAMLEAD) - [дивитися на ГітХаб](https://github.com/BoiarkinaOryna)
>- Філинська Дар'я - [дивитися на ГітХаб](https://github.com/DariaFilinskaya)
>- Гомельська Вікторія -  [дивитися на ГітХаб](https://github.com/Viktoria0228)
>- Бурлакова Аліса - [дивитися на ГітХаб](https://github.com/archalice)
>- Пілат Віктор - [дивитися на ГітХаб](https://github.com/VictorPilat)

## Що робить наш проект

#### Наш проект - це інтернет-магазин з інтегрованим Телеграм-ботом, призначений для надання інформації про товари та управління користувачами, зокрема додавання і видалення покупців, а також надання прав адміністратора.


Наш проект створений для _легкого_ доступу до товарів із _зрозумілим_ інтерфейсом та можливістю купівлі обраних продуктів. Сайт, також, має адмін-панель, що набагато облегшує _редагування, додавання та видалення_ товарів.

Для нас цей проект має велике значення, як до програмістів-початківців, тим, що ми об'єднали різні технології для його реалізування. Такі як, _Flask, GitHub, SQLite3, pandas_, telebot, HTML, CSS, та Javascript.

### Для запуску проекту необхідно завантажити :

- **Flask** - створення структури сайту
- **pandas**- завантаження товарів з таблиці
- **Flask-Login** - додавання аутентифікації до сайту
- **Flask-Migrate** - прив'язування бази даних до до проекту
- **Flask-SQLALchemy** - працює з базою данних sql3
- **Flask-Mail** - відправка email
- **telebot** - створення тулеграм-боту
- **Jinja2** - шаблонізація веб-застосунку
- **requests** - робота з http запитами


#### Щоб запустити проект локально, необхідно відкрити проект у додатку VSCode або іншому редакторі для коду та запустити файл manage.py

#### Щоб запустити проект віддалено, використайте сервер [pythonanywhere](https://www.pythonanywhere.com) для Flask та Django проектів

## Проект містить такі застосунки:

- **Home** - домашня сторінка
- **Registration** - сторінка реєстрації
- **Authorization** - сторінка для входу в акаунт
- **Shop** - каталог товарів
- **Cart** - перелік обраних товарів
- **Contacts** - перелік контактів-продавців
- **Admin** - для редагування товарів

### Створення шаблону проекту

Необхідно створити елемент класу Flask бібліотеки flask і задати йому необхідні параметри: 
import_name - назва файлу, у якому створено змінну, template-folder - шлях до папки templates та instance-path - абсолютний шлях до папки, у якій знаходиться даний файл.

```
shop = flask.Flask(
    import_name = "project",
    template_folder = "templates",
    instance_path = os.path.abspath(__file__ + "/..")
)
```

### Створення веб-сторінок
Створення веб-сторінок відбувається, за допомогою класу **Blueprint**, в якому задаються параметри:
name - відповідає за назву blueprint.
import_name -  відповідає за назву теки, у якій створюється певна структура.
template_folder - відповідає за теку templates, яка містить в собі html файл.
static_folder - відповідає за теку static, яка містить в собі файли css, js розширень, картинок та ін.
static_url_path - відповідає за налаштування URL шляху до статичних файлів у додатку.

```
shop = flask.Blueprint(
    name = 'shop',
    import_name = 'shop_page',
    template_folder = 'templates',
    static_folder = "static",
    static_url_path = "/shop/"
)
```

### Під'єднання Blueprint до Flask

До змінної-екземпляру класу Blueprint застосовуємо функцію  add_url_rule і задаемо параметри: --rule - доменний шлях до сторінки, view_func - функція показу сторінки, що включає render_template та methods - види запитів сторінки (Get, Post).

```
home_page.home.add_url_rule(
    rule = "/",
    view_func = home_page.render_home_page,
    methods = ["GET", "POST"]
)

```

До змінної-екземпляру класу Flask застосовуємо функцію register_blueprint з параметром blueprint - екземпляр класу Blueprint.

```
shop.register_blueprint(blueprint = home_page.home)
```

### Застосування форм

```
<form method="post" class="form"> 
    <p class="modal-text">CHANGE TEXT:</p>
    
    <input class = "modal-input" id = "select-file" type="" accept="" enctype = ''>
    <button name="change" type="submit" value="button-value" class="name-button">SEND</button>
</form>
```

У формі має бути параметр method зі значенням post. Всередину додаємо теги input або textarea (для введення значень) та button (для надсилання форми). За бажанням додаємо текстові теги, щоб показати, яке саме значення має приймати input.

### Завантаження сторінки

Для кожної сторінки написаний окремий файл views.py, що відповідає за показ сторінки.
У views існує функція _render_, що викликається кожного разу, як сторінка оновлюється.
Це, здебільшого, написано з метою відповіді на форми та завантаження актуального контенту з баз даних.

```
import flask
import flask_login
from registration_page.models import User
from project.settings import DB
from flask_login import current_user
from registration_page.views import is_registered

def render_authorization_page():
    if flask.request.method == "POST":
        for user in User.query.filter_by(name = flask.request.form.get("name")):
            if user.password == flask.request.form['password']:
                flask_login.login_user(user)
                return flask.redirect("/")
                    
    return flask.render_template(template_name_or_list = "authorization.html", is_registered = is_registered)
```

Це файл для сторінки **авторизації**.
Тут ми перевіряємо, чи сторінка отримала запрос post,
і тоді перевіряємо правильність введеного пароля. Якщо пароль співпвав, то користувач входить у свою сесію.

```
import flask, flask_login
from .models import User
from project.settings import DB
from project.settings import shop

is_registered = False
def render_registration_page():
    global is_registered
    # print("User.query in reg: ", User.query.all())
    print("flask.request.method =", flask.request.method)
    
    if flask.request.method == "POST":
        print("Post")
        if flask.request.form['password1'] == flask.request.form['password2']:   
            users = User( 
                name = flask.request.form['name'],
                email = flask.request.form['email'],
                password = flask.request.form['password1'],
                password_confirmation = flask.request.form["password2"]
            )
            print("12")
            if flask_login.current_user.is_authenticated:
                # print("Ви вже авторизовані")
                return "Ви вже авторизовані"
            else:
                for user in User.query.filter_by(name = flask.request.form['name']):
                    if user.password == flask.request.form['password1']:
                        flask_login.login_user(user)
                        # return flask.redirect("/")
            try:
                DB.session.add(users)
                DB.session.commit()
                print("user registered")
                is_registered = True
                # return flask.redirect("/")
                
            except:
                return "ERROR"
            else:
                pass
    return flask.render_template(template_name_or_list = "registration.html", is_registered = is_registered)

```

На сторінці **реєстрації** спершу створена змінна-флаг is_registered, що потім передається в html та показує, чи користувач зайшов на сесію, чи ні.
Вже після цього ми перевіряємо сайт на наявність відправленого post-запиту.
Створюємо екземпляр моделі для користувачів і записуємо у нього дані, введені у input форми.
У наступній умові ми перевіряємо, чи користувач вже у сессії й повертає _"Ви вже авторизовані"_, якщо умова правдива.
Далі перевіряємо правильність пароля й оновлюємо базу даних.

```
import flask, pandas, os, flask_login
from project.settings import DB
from .models import Product
def render_shop_page():
    # Product.query.delete()

    print("Product.query.all():", Product.query.all)
    # if Product.query.all() == :
    if True:
        print("1")
        path_xlsx = os.path.abspath(__file__  + "/../phones_flask_project.xlsx")
        print("2")
        read_xlsx = pandas.read_excel(
            io = path_xlsx,
            header = None,
            names = ["name", "price", "description", "count", "image", "discount"]
        )

        Product.query.delete()
        print(read_xlsx)
        for row in read_xlsx.iterrows():
            row_data = row[1]
            # print(row_data)
            product = Product(
                name = row_data['name'],
                price = row_data['price'],
                description = row_data['description'],
                count = row_data['count'],
                image = row_data['image'],
                discount = row_data["discount"]
            )
            print("product =", product)
            DB.session.add(product)

        DB.session.commit()

    return flask.render_template(
        template_name_or_list = "shop.html",
        products = Product.query.all(),
        is_authenticated = flask_login.current_user.is_authenticated
    )  

```

Для сторінки **магазину**, спочатку ми перевіряємо наявнісь продуктів у базі даних, якщо вона пуста, то завантажуємо товари з таблиці.

```
import flask, os, flask_login
from shop_page.models import Product
from project.settings import DB
def render_admin_page():
    if flask.request.method == "POST":
        print("flask.request.form.get('change') =", flask.request.form.get("change"))
        try:
            print(flask.request.form.get("del"))
            if flask.request.form.get("change"):
                list_new_value = flask.request.form.get("change").split("^")
                # print("new_value.split[-1] = ",list_new_value[2])
                print("new_value.split('^') = ", list_new_value)
                product = Product.query.get(list_new_value[1])
                print(product)
                if len(list_new_value) == 2:
                    product.image = list_new_value[0]
                elif list_new_value[-1] == "name":
                    product.name = list_new_value[0]
                elif list_new_value[-1] == "price":
                    product.price = list_new_value[0]
                elif list_new_value[-1] == "discount":
                    product.discount = list_new_value[0]
                
                DB.session.commit()
                
                
            elif flask.request.form.get("del") == None:
                print("db new product")
                product = Product(
                    image = flask.request.form["image"],
                    name = flask.request.form["name"],
                    description = flask.request.form["description"],
                    price = flask.request.form["price"],
                    discount = flask.request.form["discount"],
                    count = flask.request.form["count"]
                )
                DB.session.add(product)
                DB.session.commit()

                image = flask.request.files["image"]
                image.save(os.path.abspath(__file__ + f"/../../shop_page/static/images/{product.name}.jpg"))
            else:
                # pass
                product_id = int(flask.request.form["del"])
                product_del = Product.query.get(product_id)
                # print("product_del:", product_del)
                if Product.query.get(product_id):
                    DB.session.delete(product_del)
                    DB.session.commit()
                    os.remove(os.path.abspath(__file__ + f"/../../shop_page/static/images/{product_del.name}.jpg"))

        except Exception as e:
            print(e)
        
    return flask.render_template(template_name_or_list = "admin.html", products = Product.query.all(), is_authenticated = flask_login.current_user.is_authenticated)
```

Для **адмін-сторінки**, ми задаємо перевірку на наявність post-запросу від конкретнї форми зміну товару.
За умови, що ми відправляємо форму для зміни інформації про товар, ми оновлюємо базу даних.
Наступна умова відповідає за форму додавання продукту.
Якщо умова дійсна, то ми додаємо товар до бази.
Остання умова видаляє товар.

### Створення моделей у базі даних 

```
from project.settings import DB
class Product(DB.Model):
    id = DB.Column(DB.Integer, primary_key = True)
    name = DB.Column(DB.String(20), nullable = False)
    price = DB.Column(DB.Integer, nullable = False)
    description = DB.Column(DB.Text, nullable = False)
    count = DB.Column(DB.Integer, nullable = False)
    image = DB.Column(DB.String(100), nullable = False)
    discount = DB.Column(DB.Integer, nullable = False)

    def __repr__(self):
      return f"id - {self.id}; Назва товару - {self.name}"

```

У **shop_page** ми використовуємо models для того, щоб створити модель таблиці у базі даних. Для початку ми створюємо клас Product, в якому задаєио параметри для нього: id, name, price, description, count, image, discount. Далі до цих параметрів ми задаємо обмеження в кількості символів при введенні. У таблиці ж ці параметри мають вигляд стовпців.

```
from project.settings import DB
from flask_login import UserMixin
class User(DB.Model, UserMixin):
    id = DB.Column(DB.Integer, primary_key = True)
    name = DB.Column(DB.String(30), nullable = False)
    email = DB.Column(DB.String(70), nullable = False)
    password = DB.Column(DB.String(30), nullable = False)
    password_confirmation = DB.Column(DB.String(30), nullable = False)

    def __repr__(self):
      return f"id користувача - {self.id}; Ім'я користувача - {self.name}; Email користувача {self.email}; Пароль користувача {self.email}"

```

У **registration_page** ми використовуємо models для того, щоб створювати таблицю для усіх користувачів, що реєструються. Для початку ми створюємо клас User, в якому задаємо параметри для нього: id, name, email, password, password_confirmation. У таблиці ці параметри мають вигляд стовпців.


### Ініціалізація бази даних проекта, проведення міграцій
    flask --app settings db init
    flask --app settings db migrate
    flask --app settings db upgrade
    
    
### База даних
**База даних** - це структура, яка призначена для зберігання, зміни та обробки інформації. 

**SQLite3** - це бібліотека, яка зручно написана.
**Id** є єдиним і неповторним, у кожного id свій унікальний ключ. Оскільки кожен id є унікальним - це найвірніший спосіб знайти конкретний рядок у таблиці. Якщо назва та інші стовпці можуть бути ідентичними, то через них перевіряти ми не можемо



### Java Script

У **buttonsCookies** ми добавляємо і видаляємо товар при натиснині на кнопку. 

```
const listButtonsMinus = document.querySelectorAll(".minus")

for (let count = 0; count < listButtonsMinus.length; count++){
    let button = listButtonsMinus[count]
    button.addEventListener(
        type = "click",
        listener = function(event) {
            let listIdProducts = document.cookie.split("=")[1].split(" ")

            for(let index = 0; index < listIdProducts.length; index++){
                if (listIdProducts[index] == button.id){
                    if (button.nextElementSibling.textContent > 1){
                        listIdProducts.splice(index, 1)
                        button.nextElementSibling.textContent = +button.nextElementSibling.textContent - 1
                        console.log(listIdProducts)

                        break
                    }
                }
            }
            document.cookie = `products=${listIdProducts.join(" ")}; path = /`

        }
    )
}

const listButtonPlus = document.querySelectorAll(".plus")

for(let count = 0; count < listButtonPlus.length; count++){
    // console.log(listButtonPlus)
    let button = listButtonPlus[count]
    button.addEventListener(
        type = "click",
        listener = function(event) {
            if (document.cookie == ""){
                document.cookie = `products = ${button.id}`
                button.previousElementSibling.textContent = +button.previousElementSibling.textContent + 1
                // console.log("1", button.id)
            }else{
                // console.log("2", button.id)
                let currentProduct = document.cookie.split("=")[1]
                document.cookie = `products = ${currentProduct} ${button.id}; path = /`
                button.previousElementSibling.textContent = +button.previousElementSibling.textContent + 1
                
            }
        }
    )
}


let listDeleteProducts = document.querySelectorAll(".delete")

for (let id = 0; id < listDeleteProducts.length; id++){
    console.log(listDeleteProducts)
    let button = listDeleteProducts[id]
    button.addEventListener(
        type = "click",
        listener = function(event){
           if (document.cookie != "") {
                // let buttonsID = button.id.split("-")[1]
                // console.log(button.id)

                let listIDProducts = document.cookie.split("=")[1].split(" ")
                let lenList = listIDProducts.length

                for (let a = 0; a < lenList; a++) {
                    for (let c = 0; c < lenList; c++) {
                        if (listIDProducts[c] === button.id) {
                            // console.log(c)
                            // console.log(lenList)
                            listIDProducts.splice(c, 1)
                            console.log(listIDProducts)
                        }
                    }
                }   
                // console.log(document.querySelector(`.delete`))
                console.log(document.getElementById(`${button.id}`))
                // console.log(document.querySelector(`.products`))
                document.getElementById(button.id).remove()
                // document.querySelector(".products").remove()
                // document.cookie = `products=${listIdProducts.join(" ")}; path = /`
                if (listIDProducts.length >= 0) {
                    // console.log(listIDProducts)
                    document.cookie = `products = ${listIDProducts.join(' ')}; path = /` 
                    // document.cookie = `products = ; path = /` 
                }
                if (document.cookie.split('=')[1] == ''){
                    let p = document.createElement("p")
                    p.textContent = "Корзина порожня😢"
                    document.body.append(p)
                    document.querySelector(".order").remove()
                }
            }
        }
    )
}

```

У **productInfo** функція викликаєтся кожного разу, як сторінка оновлюється. 
Вона оброблює кукі-файли та виводить на екран усі обрані товари та їхню кількість.

```
document.addEventListener(
    type = "DOMContentLoaded",
    listener = function(event){
        let listCountProducts = document.querySelectorAll('.products')
        let productCountId = document.getElementById('count-product')
        // let totalPriceId = document.getElementById('total-price')
        productCountId.textContent = listCountProducts.length
        // let listNumberPrices = []
        let totalPrice = 0
        let totalDiscount = 0
        
        // let listCountCount = []

        // let product = products[productPrice]
        let listProductPrices = document.querySelectorAll(".product-price")
        for(let productPrice = 0; productPrice < listProductPrices.length; productPrice++ ){
            console.log(listProductPrices[productPrice])
            // console.log("innerText =", productPrice.textContent)

            let listFullPrice = document.querySelectorAll(".invisible")
            for (fullPrice of listFullPrice){
                console.log("fullPrice.innerText =", fullPrice.textContent, "; Number(listProductPrices[productPrice].innerText.split()[0] =", Number(listProductPrices[productPrice].innerText.split(" ")[0]))
                let onlyNumberPrice = Number(fullPrice.textContent).toFixed(2)
                let elementId = listProductPrices[productPrice].id
                console.log("id =", elementId, document.getElementById(`quantity ${elementId}`).textContent)

                // listNumberPrices.push(onlyNumberPrice * document.getElementById(`quantity ${elementId}`).textContent)
                let quantity = Number(document.getElementById(`quantity ${elementId}`).textContent)
                let totalPriceResult = onlyNumberPrice * quantity

                // listNumberPrices.push(totalPriceResult.toFixed(2))
                totalPrice += totalPriceResult
                if (fullPrice.id == listProductPrices[productPrice].id){
                    discount = Number(fullPrice.textContent) - Number(listProductPrices[productPrice].innerText.split(" ")[0])
                    console.log("discount =", discount)
                    for (let count = 0; count < quantity; count++) {
                        totalDiscount += discount
                    }
                }
                
            }
            console.log(totalDiscount)
            document.getElementById("total-discount").innerText = `${totalDiscount.toFixed(2)} грн`
            
        }
        console.log("total price:", totalPrice)
        document.getElementById("total-price").textContent = `${totalPrice.toFixed(2)} `
        

        let totalTotalPrice = (totalPrice) - totalDiscount
        document.querySelector("#total-amount").textContent = `${totalTotalPrice.toFixed(2)} грн`
    }
)

let listPrice = document.querySelectorAll(".product-price")

for (let price of listPrice){
    console.log("price =", price)
    price.innerText = `${Number(price.innerText.split(" ")[0]).toFixed(2)} грн`
    console.log("price.innerText =", price.innerText)
}

let checkoutButton = document.querySelector(".checkout")


checkoutButton.addEventListener(
    type = "click",
    listener = function(event){
        event.preventDefault()
        document.querySelector(".placing").style.display = "flex"
        console.log("aaskasjinaso")
    }
)

let sendButton = document.createElement(".send-product")

sendButton.addEventListener(
    type = "click",
    listener = function(event){
        let all = document.querySelectorAll()
        all.remove()
        console.log("a")

        let h2 = document.createElement("h2")
        h2.classList.add("waiting-text")
        document.body.appendChild(h2)
    }
)

```

У **admin** ми створюємо дії, що відтворюється при натисканні на кнопки зміни інформації про товар та поява форми для додавання нових товарів у базу.

```
let listPencilName = document.querySelectorAll(".pencil-name")
let listPencilDiscount = document.querySelectorAll(".pencil-discount")
let listPencilPrice = document.querySelectorAll(".pencil-price")
let listPencilDescription = document.querySelectorAll(".pencil-description")
let listPencilImage= document.querySelectorAll(".pencil-image")
let myForm = document.querySelector(".form")

function addListButton(classList, element) {
  for (let i = 0; i < classList.length; i++) {
      let currentElement = classList[i]
      currentElement.addEventListener(
        type = "click",
        listener = function(event){
          ChangeButton(event, element, i)
        } 
      )
  }          
}

addListButton(listPencilName, 'name')
addListButton(listPencilDiscount, 'discount')
addListButton(listPencilDescription, 'decription')
addListButton(listPencilPrice, 'price')

function ChangeButton(event, element, index){
  let listButtonPencil = []
  if (element == 'name'){
    listButtonPencil = listPencilName
  }
  else if (element == 'discount'){
    listButtonPencil = listPencilDiscount
  }
  else if (element == 'description'){
    listButtonPencil = listPencilDescription
  }
  else if (element == 'price'){
    listButtonPencil = listPencilPrice
  }

  idPencil = listButtonPencil[index].id
  console.log(listButtonPencil[index].id)
  let input = document.querySelector(".modal-input")
  input.type = 'text'
  input.id = idPencil
  console.log(input)
  console.log(myForm)
  // myForm.append(input)
  document.querySelector('.modal-window').style.display = 'flex'
  let button = document.querySelector(".name-button")
  button.id = idPencil
  button.style.display = "flex"
  button.addEventListener(
      type = "click",
      listener = function(event) {
          let newValue = input.value
          let elementValue = document.getElementById(`${element}-${idPencil}`)
          console.log(elementValue, `${element}-${idPencil}`)
          button.value = newValue + "^" + button.id + "^" + element
          elementValue.textContent = newValue
          console.log(newValue)
          input.remove()
          // button.style.display = none 
      }
  )
}

for (let i = 0; i < listPencilImage.length; i++) {
  let pencilImage = listPencilImage[i]
  pencilImage.addEventListener(
    type = 'click', 
    listener = (event) =>{
      let id = pencilImage.id
      console.log(id)
      let input = document.querySelector(".modal-input")
      input.accept = 'image/*'
      input.type = "file"
      input.enctype = 'multipart/form-data'
      input.id = id


      document.querySelector(".modal-text").textContent = "CHANGE IMAGE"
      document.querySelector('.modal-window').style.display = 'flex'
      button = document.querySelector(".name-button")
      button.addEventListener(
        type = "click",
        listener = (event)=>{
          let newValue = input.value.split(/\\/g)[input.value.split(/\\/g).length - 1]
          button.value = newValue + "^" + id
        }
      )
    }
  )
}

let addProductButton = document.querySelector(".plus")

addProductButton.addEventListener(
  type = "click",
  listener = (event) =>{
    console.log("add new product")
    let addProductForm = document.querySelector(".modal-form")
    addProductForm.style.display = "flex"
  }
)

```


У **authoritzation.js** Ми робимо так щоб цей код перевіряв чи натиснута кнопка SEND перш ніж показувати модальне вікно.

```
let buttonSend = document.querySelector(".authorization_submit")
let regForm = document.querySelector("#authorization_content")

buttonSend.addEventListener(
  type = "click",
  listener = function(event){
    let inputsFilled = true
    // event.preventDefault()
    console.log("button send.clicked")
    for (let index = 0; index < regForm.elements.length; index++) { 
      let element = regForm.elements[index]; 
      if (!element.value){
        inputsFilled = false
       }
      if(inputsFilled){
        document.querySelector(".modal-window").style.display = "flex"
      }
    // event.preventDefault()
    } 
  }
)
```

```
let buttonSubmit = document.querySelector(".registration_submit")
let regForm = document.querySelector("#registration_content")
buttonSubmit.addEventListener(
  type = "click",
  listener = function(event){
    let inputsFilled = true
    // event.preventDefault()
    for (let index = 0; index < regForm.elements.length; index++) { 
      let element = regForm.elements[index]; 
      if (!element.value){
        inputsFilled = false
       }
      if(inputsFilled){
        document.querySelector(".modal-window").style.display = "flex"
      }
    } 
  }
)

```



# Internet Shop

## List of team members

>- Boiarkina Oryna (TEAMLEAD) - [view on GitHub](https://github.com/BoiarkinaOryna) 
>- Filynska Daria - [view on GitHub](https://github.com/DariaFilinskaya)
>- Homelska Viktoria - [view on GitHub](https://github.com/Viktoria0228)
>- Burlakova Alisa - [view on GitHub](https://github.com/archalice)
>- Pilat Viktor - [view on GitHub](https://github.com/VictorPilat)


## What our project does

#### Our project is an online store with an integrated Telegram bot, designed to provide information about products and user management, including adding and removing buyers, as well as providing administrator rights.


Our project was created for _easy_ access to goods with an _understandable_ interface and the ability to buy selected products. The site also has an admin panel that makes _editing, adding and removing_ products much easier.

For us, this project is of great importance, as for beginner programmers, because we have combined various technologies for its implementation. Such as _Flask, GitHub, SQLite3, pandas_, telebot, HTML, CSS, and Javascript.


### To start the project, you need to download:

- **Flask** - creation of site structure
- **pandas** - loading products from the table
- **Flask-Login** - adding authentication to the site
- **Flask-Migrate** - binding the database to the project
- **Flask-SQLALchemy** - works with sql3 database
- **Flask-Mail** - sending email
- **telebot** - creation of a telegram bot
- **Jinja2** - web application templating
- **requests** - work with http requests


#### To run the project remotely, use the [pythonanywhere](https://www.pythonanywhere.com) server for Flask and Django projects

## The project contains the following applications:

- **Home** - home page
- **Registration** - registration page
- **Authorization** - account login page
- **Shop** - product catalog
- **Cart** - list of selected products
- **Contacts** - list of seller contacts
- **Admin** - for editing products


### Creating a project template

It is necessary to create an element of the Flask class of the flask library and set the necessary parameters to it: 
import_name - the name of the file in which the variable was created, template-folder - the path to the templates folder, and instance-path - the absolute path to the folder in which this file is located.

```
shop = flask.Flask(
    import_name = "project",
    template_folder = "templates",
    instance_path = os.path.abspath(__file__ + "/..")
)
```


### Application of forms

```
<form method="post" class="form"> 
    <p class="modal-text">CHANGE TEXT:</p>
    
    <input class = "modal-input" id = "select-file" type="" accept="" enctype = ''>
    <button name="change" type="submit" value="button-value" class="name-button">SEND</button>
</form>
```

The form must have a method parameter with the value post. Inside, we add the input or textarea (for entering values) and button (for sending the form) tags. Optionally, we add text tags to show exactly what value input should take.

### Loading page

A separate views.py file is written for each page, which is responsible for displaying the page.
In views there is a _render_ function that is called every time the page is refreshed.
It is mostly written to respond to forms and download relevant content from databases.

```
import flask, pandas, os, flask_login
from project.settings import DB
from .models import Product
def render_shop_page():
    # Product.query.delete()

    print("Product.query.all():", Product.query.all)
    # if Product.query.all() == :
    if True:
        print("1")
        path_xlsx = os.path.abspath(__file__  + "/../phones_flask_project.xlsx")
        print("2")
        read_xlsx = pandas.read_excel(
            io = path_xlsx,
            header = None,
            names = ["name", "price", "description", "count", "image", "discount"]
        )

        Product.query.delete()
        print(read_xlsx)
        for row in read_xlsx.iterrows():
            row_data = row[1]
            # print(row_data)
            product = Product(
                name = row_data['name'],
                price = row_data['price'],
                description = row_data['description'],
                count = row_data['count'],
                image = row_data['image'],
                discount = row_data["discount"]
            )
            print("product =", product)
            DB.session.add(product)

        DB.session.commit()

    return flask.render_template(
        template_name_or_list = "shop.html",
        products = Product.query.all(),
        is_authenticated = flask_login.current_user.is_authenticated
    )  

```

For the **shop** page, first we check the available products in the database, if it is empty, then we load the products from the table.

```
import flask, os, flask_login
from shop_page.models import Product
from project.settings import DB
def render_admin_page():
    if flask.request.method == "POST":
        print("flask.request.form.get('change') =", flask.request.form.get("change"))
        try:
            print(flask.request.form.get("del"))
            if flask.request.form.get("change"):
                list_new_value = flask.request.form.get("change").split("^")
                # print("new_value.split[-1] = ",list_new_value[2])
                print("new_value.split('^') = ", list_new_value)
                product = Product.query.get(list_new_value[1])
                print(product)
                if len(list_new_value) == 2:
                    product.image = list_new_value[0]
                elif list_new_value[-1] == "name":
                    product.name = list_new_value[0]
                elif list_new_value[-1] == "price":
                    product.price = list_new_value[0]
                elif list_new_value[-1] == "discount":
                    product.discount = list_new_value[0]
                
                DB.session.commit()
                
                
            elif flask.request.form.get("del") == None:
                print("db new product")
                product = Product(
                    image = flask.request.form["image"],
                    name = flask.request.form["name"],
                    description = flask.request.form["description"],
                    price = flask.request.form["price"],
                    discount = flask.request.form["discount"],
                    count = flask.request.form["count"]
                )
                DB.session.add(product)
                DB.session.commit()

                image = flask.request.files["image"]
                image.save(os.path.abspath(__file__ + f"/../../shop_page/static/images/{product.name}.jpg"))
            else:
                # pass
                product_id = int(flask.request.form["del"])
                product_del = Product.query.get(product_id)
                # print("product_del:", product_del)
                if Product.query.get(product_id):
                    DB.session.delete(product_del)
                    DB.session.commit()
                    os.remove(os.path.abspath(__file__ + f"/../../shop_page/static/images/{product_del.name}.jpg"))

        except Exception as e:
            print(e)
        
    return flask.render_template(template_name_or_list = "admin.html", products = Product.query.all(), is_authenticated = flask_login.current_user.is_authenticated)
```


For the **admin page**, we set a check for the presence of a post-request from a specific product change form.
Provided that we submit a form to change product information, we update the database.
The following condition is responsible for the product addition form.
If the condition is valid, then we add the product to the database.
The last condition removes the item.


### Creation of models in the database 

```
from project.settings import DB
class Product(DB.Model):
    id = DB.Column(DB.Integer, primary_key = True)
    name = DB.Column(DB.String(20), nullable = False)
    price = DB.Column(DB.Integer, nullable = False)
    description = DB.Column(DB.Text, nullable = False)
    count = DB.Column(DB.Integer, nullable = False)
    image = DB.Column(DB.String(100), nullable = False)
    discount = DB.Column(DB.Integer, nullable = False)

    def __repr__(self):
      return f"id - {self.id}; Product name - {self.name}"

```

In **shop_page** we use models to create a table model in the database. To begin with, we create a Product class, in which we set parameters for it: id, name, price, description, count, image, discount. Next, to these parameters, we set a limit on the number of characters when entering. In the table, these parameters have the form of columns.

```
from project.settings import DB
from flask_login import UserMixin
class User(DB.Model, UserMixin):
    id = DB.Column(DB.Integer, primary_key = True)
    name = DB.Column(DB.String(30), nullable = False)
    email = DB.Column(DB.String(70), nullable = False)
    password = DB.Column(DB.String(30), nullable = False)
    password_confirmation = DB.Column(DB.String(30), nullable = False)

    def __repr__(self):
      return f"user id - {self.id}; User name - {self.name}; User email {self.email}; User password {self.email}"

```


In the **registration_page** we use models to create a table for all users registering. To begin with, we create the User class, in which we set the parameters for it: id, name, email, password, password_confirmation. In the table, these parameters have the form of columns.


### Initialization of the project database, migrations
    flask --app settings db init
    flask --app settings db migrate
    flask --app settings db upgrade
    
    
### Database
**Database** is a structure designed to store, change and process information. 

**SQLite3** is a library that is conveniently written.
**Id** is unique and unique, each id has its own unique key. Since each id is unique, this is the most reliable way to find a specific row in the table. If the name and other columns can be identical, then we cannot check through them.A


### Java Script

In **buttonsCookies** we add and remove a product when a button is pressed. 

```
const listButtonsMinus = document.querySelectorAll(".minus")

for (let count = 0; count < listButtonsMinus.length; count++){
    let button = listButtonsMinus[count]
    button.addEventListener(
        type = "click",
        listener = function(event) {
            let listIdProducts = document.cookie.split("=")[1].split(" ")

            for(let index = 0; index < listIdProducts.length; index++){
                if (listIdProducts[index] == button.id){
                    if (button.nextElementSibling.textContent > 1){
                        listIdProducts.splice(index, 1)
                        button.nextElementSibling.textContent = +button.nextElementSibling.textContent - 1
                        console.log(listIdProducts)

                        break
                    }
                }
            }
            document.cookie = `products=${listIdProducts.join(" ")}; path = /`

        }
    )
}

const listButtonPlus = document.querySelectorAll(".plus")

for(let count = 0; count < listButtonPlus.length; count++){
    // console.log(listButtonPlus)
    let button = listButtonPlus[count]
    button.addEventListener(
        type = "click",
        listener = function(event) {
            if (document.cookie == ""){
                document.cookie = `products = ${button.id}`
                button.previousElementSibling.textContent = +button.previousElementSibling.textContent + 1
                // console.log("1", button.id)
            }else{
                // console.log("2", button.id)
                let currentProduct = document.cookie.split("=")[1]
                document.cookie = `products = ${currentProduct} ${button.id}; path = /`
                button.previousElementSibling.textContent = +button.previousElementSibling.textContent + 1
                
            }
        }
    )
}


let listDeleteProducts = document.querySelectorAll(".delete")

for (let id = 0; id < listDeleteProducts.length; id++){
    console.log(listDeleteProducts)
    let button = listDeleteProducts[id]
    button.addEventListener(
        type = "click",
        listener = function(event){
           if (document.cookie != "") {
                // let buttonsID = button.id.split("-")[1]
                // console.log(button.id)

                let listIDProducts = document.cookie.split("=")[1].split(" ")
                let lenList = listIDProducts.length

                for (let a = 0; a < lenList; a++) {
                    for (let c = 0; c < lenList; c++) {
                        if (listIDProducts[c] === button.id) {
                            // console.log(c)
                            // console.log(lenList)
                            listIDProducts.splice(c, 1)
                            console.log(listIDProducts)
                        }
                    }
                }   
                // console.log(document.querySelector(`.delete`))
                console.log(document.getElementById(`${button.id}`))
                // console.log(document.querySelector(`.products`))
                document.getElementById(button.id).remove()
                // document.querySelector(".products").remove()
                // document.cookie = `products=${listIdProducts.join(" ")}; path = /`
                if (listIDProducts.length >= 0) {
                    // console.log(listIDProducts)
                    document.cookie = `products = ${listIDProducts.join(' ')}; path = /` 
                    // document.cookie = `products = ; path = /` 
                }
                if (document.cookie.split('=')[1] == ''){
                    let p = document.createElement("p")
                    p.textContent = "Корзина порожня😢"
                    document.body.append(p)
                    document.querySelector(".order").remove()
                }
            }
        }
    )
}

```




```
document.addEventListener(
    type = "DOMContentLoaded",
    listener = function(event){
        let listCountProducts = document.querySelectorAll('.products')
        let productCountId = document.getElementById('count-product')
        // let totalPriceId = document.getElementById('total-price')
        productCountId.textContent = listCountProducts.length
        // let listNumberPrices = []
        let totalPrice = 0
        let totalDiscount = 0
        
        // let listCountCount = []

        // let product = products[productPrice]
        let listProductPrices = document.querySelectorAll(".product-price")
        for(let productPrice = 0; productPrice < listProductPrices.length; productPrice++ ){
            console.log(listProductPrices[productPrice])
            // console.log("innerText =", productPrice.textContent)

            let listFullPrice = document.querySelectorAll(".invisible")
            for (fullPrice of listFullPrice){
                console.log("fullPrice.innerText =", fullPrice.textContent, "; Number)(listProductPrices[productPrice].innerText.split()[0] =", Number(listProductPrices[productPrice].innerText.split(" ")[0]))
                let onlyNumberPrice = Number(fullPrice.textContent).toFixed(2)
                let elementId = listProductPrices[productPrice].id
                console.log("id =", elementId, document.getElementById(`quantity ${elementId}`).textContent)

                // listNumberPrices.push(onlyNumberPrice * document.getElementById(`quantity ${elementId}`).textContent)
                let quantity = Number(document.getElementById(`quantity ${elementId}`).textContent)
                let totalPriceResult = onlyNumberPrice * quantity

                // listNumberPrices.push(totalPriceResult.toFixed(2))
                totalPrice += totalPriceResult
                if (fullPrice.id == listProductPrices[productPrice].id){
                    discount = Number(fullPrice.textContent) - Number(listProductPrices[productPrice].innerText.split(" ")[0])
                    console.log("discount =", discount)
                    for (let count = 0; count < quantity; count++) {
                        totalDiscount += discount
                    }
                }
                
            }
            console.log(totalDiscount)
            document.getElementById("total-discount").innerText = `${totalDiscount.toFixed(2)} грн`
            
        }
        console.log("total price:", totalPrice)
        document.getElementById("total-price").textContent = `${totalPrice.toFixed(2)} `
        

        let totalTotalPrice = (totalPrice) - totalDiscount
        document.querySelector("#total-amount").textContent = `${totalTotalPrice.toFixed(2)} грн`
    }
)

let listPrice = document.querySelectorAll(".product-price")

for (let price of listPrice){
    console.log("price =", price)
    price.innerText = `${Number(price.innerText.split(" ")[0]).toFixed(2)} грн`
    console.log("price.innerText =", price.innerText)
}

let checkoutButton = document.querySelector(".checkout")


checkoutButton.addEventListener(
    type = "click",
    listener = function(event){
        event.preventDefault()
        document.querySelector(".placing").style.display = "flex"
        console.log("aaskasjinaso")
    }
)

let sendButton = document.createElement(".send-product")

sendButton.addEventListener(
    type = "click",
    listener = function(event){
        let all = document.querySelectorAll()
        all.remove()
        console.log("a")

        let h2 = document.createElement("h2")
        h2.classList.add("waiting-text")
        document.body.appendChild(h2)
    }
)

```



In **admin**, we create actions that are played when clicking on the buttons to change product information and the appearance of a form for adding new products to the database.

```
let listPencilName = document.querySelectorAll(".pencil-name")
let listPencilDiscount = document.querySelectorAll(".pencil-discount")
let listPencilPrice = document.querySelectorAll(".pencil-price")
let listPencilDescription = document.querySelectorAll(".pencil-description")
let listPencilImage= document.querySelectorAll(".pencil-image")
let myForm = document.querySelector(".form")

function addListButton(classList, element) {
  for (let i = 0; i < classList.length; i++) {
      let currentElement = classList[i]
      currentElement.addEventListener(
        type = "click",
        listener = function(event){
          ChangeButton(event, element, i)
        } 
      )
  }          
}

addListButton(listPencilName, 'name')
addListButton(listPencilDiscount, 'discount')
addListButton(listPencilDescription, 'decription')
addListButton(listPencilPrice, 'price')

function ChangeButton(event, element, index){
  let listButtonPencil = []
  if (element == 'name'){
    listButtonPencil = listPencilName
  }
  else if (element == 'discount'){
    listButtonPencil = listPencilDiscount
  }
  else if (element == 'description'){
    listButtonPencil = listPencilDescription
  }
  else if (element == 'price'){
    listButtonPencil = listPencilPrice
  }

  idPencil = listButtonPencil[index].id
  console.log(listButtonPencil[index].id)
  let input = document.querySelector(".modal-input")
  input.type = 'text'
  input.id = idPencil
  console.log(input)
  console.log(myForm)
  // myForm.append(input)
  document.querySelector('.modal-window').style.display = 'flex'
  let button = document.querySelector(".name-button")
  button.id = idPencil
  button.style.display = "flex"
  button.addEventListener(
      type = "click",
      listener = function(event) {
          let newValue = input.value
          let elementValue = document.getElementById(`${element}-${idPencil}`)
          console.log(elementValue, `${element}-${idPencil}`)
          button.value = newValue + "^" + button.id + "^" + element
          elementValue.textContent = newValue
          console.log(newValue)
          input.remove()
          // button.style.display = none 
      }
  )
}

for (let i = 0; i < listPencilImage.length; i++) {
  let pencilImage = listPencilImage[i]
  pencilImage.addEventListener(
    type = 'click', 
    listener = (event) =>{
      let id = pencilImage.id
      console.log(id)
      let input = document.querySelector(".modal-input")
      input.accept = 'image/*'
      input.type = "file"
      input.enctype = 'multipart/form-data'
      input.id = id


      document.querySelector(".modal-text").textContent = "CHANGE IMAGE"
      document.querySelector('.modal-window').style.display = 'flex'
      button = document.querySelector(".name-button")
      button.addEventListener(
        type = "click",
        listener = (event)=>{
          let newValue = input.value.split(/\\/g)[input.value.split(/\\/g).length - 1]
          button.value = newValue + "^" + id
        }
      )
    }
  )
}

let addProductButton = document.querySelector(".plus")

addProductButton.addEventListener(
  type = "click",
  listener = (event) =>{
    console.log("add new product")
    let addProductForm = document.querySelector(".modal-form")
    addProductForm.style.display = "flex"
  }
)

```

In **authoritzation.js** We make this code check if the SEND button is pressed before showing the modal window.

```
let buttonSend = document.querySelector(".authorization_submit")
let regForm = document.querySelector("#authorization_content")

buttonSend.addEventListener(
  type = "click",
  listener = function(event){
    let inputsFilled = true
    // event.preventDefault()
    console.log("button send.clicked")
    for (let index = 0; index < regForm.elements.length; index++) { 
      let element = regForm.elements[index]; 
      if (!element.value){
        inputsFilled = false
       }
      if(inputsFilled){
        document.querySelector(".modal-window").style.display = "flex"
      }
    // event.preventDefault()
    } 
  }
)
```

```
let buttonSubmit = document.querySelector(".registration_submit")
let regForm = document.querySelector("#registration_content")
buttonSubmit.addEventListener(
  type = "click",
  listener = function(event){
    let inputsFilled = true
    // event.preventDefault()
    for (let index = 0; index < regForm.elements.length; index++) { 
      let element = regForm.elements[index]; 
      if (!element.value){
        inputsFilled = false
       }
      if(inputsFilled){
        document.querySelector(".modal-window").style.display = "flex"
      }
    } 
  }
)

```