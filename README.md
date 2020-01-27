# How-to-create-jump-list-using-Xamarin.Forms-listview

You can create jump list using Xamarin.Forms ListView by tap on the indexed letter group key) and scroll to the respective group [GroupHeaderItem](https://help.syncfusion.com/cr/xamarin/Syncfusion.SfListView.XForms~Syncfusion.ListView.XForms.GroupHeaderItem.html) by passing group key index in [ScrollToIndex](https://help.syncfusion.com/cr/cref_files/xamarin/Syncfusion.SfListView.XForms~Syncfusion.ListView.XForms.LayoutBase~ScrollToRowIndex.html) method like contact list. This article explains you how to create jump list with group key and scroll to tapped item.

```
<ContentPage xmlns:syncfusion="clr-namespace:Syncfusion.ListView.XForms;assembly=Syncfusion.SfListView.XForms" >
    <ContentPage.Content>
       <Grid >
          <syncfusion:SfListView x:Name="listView" Grid.Row="0">
                <syncfusion:SfListView.ItemTemplate>
                          <DataTemplate>
                                     <ViewCell>
                                             <Grid>
                                                  <Image Source="{Binding ContactImage}" Grid.Column="0"/>
                                                        <StackLayout Orientation="Vertical" Grid.Column="1">
                                                                     <Label Text="{Binding ContactName}"/>
                                                                     <Label Text="{Binding ContactNumber}"/>
                                                        </StackLayout>
                                               </Grid>
                                             <StackLayout HeightRequest="1" BackgroundColor="LightGray"/>
                                     </ViewCell>
                          </DataTemplate>
                </syncfusion:SfListView.ItemTemplate>                
            </syncfusion:SfListView>
            <Grid x:Name="IndexPanelGrid" Grid.Row="0"  VerticalOptions="CenterAndExpand" HorizontalOptions="End" />
         </Grid>
    </ContentPage.Content>
</ContentPage>
```
Creating new label with index letter from the Key value of GroupHeader after loading.

```
ListView.Loaded += ListView_Loaded;

private void ListView_Loaded(object sender, ListViewLoadedEventArgs e)
{
     var groupcount = this.ListView.DataSource.Groups.Count;
     for (int i = 0; i < groupcount; i++)
     {
        label = new Label();  
        GroupResult group = ListView.DataSource.Groups[i];
        label.Text = group.Key.ToString();
        indexPanelGrid.Children.Add(label, 0, i);
        var labelTapped = new TapGestureRecognizer() { Command = new Command<object>(OnTapped), CommandParameter = label };
        label.GestureRecognizers.Add(labelTapped);
     }
}
```
On tapping the group key value loaded in IndexLabelGrid, you can scroll the listview to the respective group by comparing all GroupHeader’s Key value like below.

```
private void OnTapped(object obj)
{
    if (previousLabel != null)
    {
      previousLabel.TextColor = Color.DimGray;
    }
    var label = obj as Label;
    var text = label.Text;
    label.TextColor = Color.Red;
    for (int i = 0; i < ListView.DataSource.Groups.Count; i++)
    {
        var group = ListView.DataSource.Groups[i];

        if (group.Key == null)
          App.Current.MainPage.DisplayAlert("Message", "Group not available", "ok");
        if ((group.Key != null && group.Key.Equals(text)))
        {
                  ListView.LayoutManager.ScrollToRowIndex (ListView.DataSource.DisplayItems.IndexOf(group), true);
         }
     }
     previousLabel = label;
}
```
