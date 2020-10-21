# TheCashier
merupakan aplikasi yang bisa digunakan untuk membantu pekerjaan kasir. menghitung jumlah suatu barang/jasa dan menjumlahkan harganya dalam Rupiah/

## Scope & Functionalities
- user dapat memasukkan nama item
- user dapat memilih tipe yang dibeli (barang/jasa)
- user dapat memasukkan jumlah item
- user dapat memasukkan harga
- user dapat melihat list item dan melihat total harganya

### How Does it Works?
terdapat class Item yang digunakan untuk logika untuk menampung Item yang dimasukkan dan akan diambil nantinya
```csharp
    class Item
    {
        private int id;
        public string title { get; set; }
        public int quantity { get; set; }
        public double price { get; set; }
        public double subtotal { get; set; }
        private string type;

        public Item(int id, string title, int quantity, string type, double price)
        {
            this.id = id;
            this.title = title;
            this.quantity = quantity;
            this.type = type;
            this.price = price;
            this.subtotal = 0;
        }
        public string getTitle()
        {
            return title;
        }
        public int getQuantity()
        {
            return quantity;
        }
        public string getType()
        {
            return type;
        }
        public double getPrice()
        {
            return price;
        }
        public double getSubTotal()
        {
            subtotal = price * quantity;
            return subtotal;
        }
    }
}
```
Lalu terdapat class Calculator sebagai logika untuk memproses penghitungan dan pengambilan data dari class Item
```csharp
    class Calculator
    {
        private List<Item> listItem;
        private double total=0;

        public Calculator()
        {
            this.listItem = new List<Item>();
        }
        public void addItem(Item item)
        {
            this.listItem.Add(item);
            this.total += item.getSubTotal();
        }
        public double getTotal()
        {
            return total;
        }

        public List<Item> getListItem()
        {
            return listItem;
        }
    }
}
```
Kemudian agar dapat tampil di mainWindow maka di MainWindow.xaml.cs di edit menjadi seperti berikut,
```
    public partial class MainWindow : Window
    {
        private Calculator calculator;
        public MainWindow()
        {
            InitializeComponent();
            calculator = new Calculator();
            listBox.ItemsSource = calculator.getListItem();
        }

        private void AddButton_Click(object sender, RoutedEventArgs e)
        {
            string title = itemNameBox.Text;
            int quantity = int.Parse(quantityBox.Text);
            string type = typeBox.Text;
            double price = double.Parse(priceBox.Text);

            Item item = new Item(new Random().Next(), title, quantity, type, price);
            calculator.addItem(item);
            double total = calculator.getTotal();

            totalLabel.Content = String.Format("Rp. {0}", total);

            listBox.Items.Refresh();
        }
    }
}
```
listBox pada coding diatas merupakan name dari ListBox di main window. Karna itu data dari Item dapat ditampilkan di dalam ListBox di main window sesuai yang telah di inputkan user sebelumnya.
Pada class Item menggunakan prinsip single responsibility sehingga hanya menyediakan nama dan jenis variabel. Untuk fungsinya terdapat pada class Calculator di listItem dan total.
