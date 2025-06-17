Aplicație WPF: Afișarea unui Grafic din Date Introduse Manual
1. Descriere
Această aplicație WPF permite introducerea unor valori numerice separate prin virgulă într-un control de tip TextBox. Utilizatorul poate selecta tipul de grafic dorit (linie sau bară), iar la apăsarea unui buton, datele sunt afișate într-un grafic corespunzător. Aplicația utilizează biblioteca LiveCharts pentru desenarea graficului.
2. Interfața Grafică (XAML)
Codul XAML pentru interfața aplicației este următorul:

<Window x:Class="GraficTextInputWPF.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:lvc="clr-namespace:LiveCharts.Wpf;assembly=LiveCharts.Wpf"
        Title="Grafic din TextBox" Height="500" Width="800">
    <Grid Margin="20">
        <StackPanel>
            <TextBlock Text="Introduceți valorile separate prin virgulă:" FontWeight="Bold"/>
            <TextBox Name="txtValori" Width="300" Height="30" Margin="0,5,0,10" />
            <TextBlock Text="Alegeți tipul de grafic:" FontWeight="Bold"/>
            <ComboBox Name="cmbTipGrafic" Width="200" Height="30" Margin="0,5,0,10">
                <ComboBoxItem Content="Linie" IsSelected="True"/>
                <ComboBoxItem Content="Bară"/>
            </ComboBox>
            <Button Content="Generează graficul" Width="200" Height="40" Click="BtnGenereaza_Click"/>
            <lvc:CartesianChart Name="grafic" Height="300" Margin="0,20,0,0"/>
        </StackPanel>
    </Grid>
</Window>


3. Codul C# (MainWindow.xaml.cs)
Codul logic pentru afișarea graficului în funcție de valorile introduse este:

using LiveCharts;
using LiveCharts.Wpf;
using System;
using System.Linq;
using System.Windows;
using System.Windows.Controls;

namespace GraficTextInputWPF
{
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
        }

        private void BtnGenereaza_Click(object sender, RoutedEventArgs e)
        {
            grafic.Series.Clear();
            try
            {
                var valori = txtValori.Text
                    .Split(',')
                    .Select(v => double.Parse(v.Trim()))
                    .ToArray();

                string tip = ((ComboBoxItem)cmbTipGrafic.SelectedItem).Content.ToString();

                if (tip == "Linie")
                {
                    grafic.Series.Add(new LineSeries
                    {
                        Title = "Valori",
                        Values = new ChartValues<double>(valori)
                    });
                }
                else if (tip == "Bară")
                {
                    grafic.Series.Add(new ColumnSeries
                    {
                        Title = "Valori",
                        Values = new ChartValues<double>(valori)
                    });
                }
            }
            catch
            {
                MessageBox.Show("Introduceți doar numere separate prin virgulă!");
            }
        }
    }
}

Prezentarea interfeței aplicației
Interfața aplicației este una intuitivă și aerisită, fiind structurată în jurul a trei elemente principale: două câmpuri de introducere a datelor numerice, un buton de selectare a tipului de grafic, precum și un buton de execuție care generează graficul în funcție de valorile introduse.
