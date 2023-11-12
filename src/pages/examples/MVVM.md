## MVVM PATTERN EXAMPLE 

The Model-View-ViewModel (MVVM) architecture pattern, also known as Model View ViewModel, refers to a design model aimed at separating the user interface (View) from the logic (Model). Its objective is to make the visual aspect completely independent.

The ViewModel, in turn, plays a prominent role as the component responsible for serving as a bridge between the interaction of the View and the Model.

To make use of the MVVM architecture pattern or MVVM model, it is crucial to understand how the code of applications is structured into appropriate classes and comprehend their interaction with design components.

**STRUCTURE**

<div align="center">
<img src="https://shirivo.files.wordpress.com/2015/06/mvvm.png" alt="STRUCTURE">
</div> 

---

The following example is taken from the "Files-community" repository. The Files repository on GitHub hosts the source code for a file management application called "Files." This application is presented as the definitive solution for file management on the Windows operating system. With an elegant and intuitive design, Files provides an exceptional user experience by simplifying file navigation and organization.

Highlighted features of Files include the ability to work with multiple tabs for easy switching between folders, a column view to quickly explore file contents, and dual-panel support for efficient management. The application also streamlines the file compression and decompression process, enabling users to create and extract files with just a few clicks.

In relation to the **MVVM design pattern**, in the repository's ViewModels folder, we can find files where the design pattern is implemented in the following structure:

- **View**:

Directories and files such as "Dialogs," "LayoutModes," "Previews," "Properties," "Settings," "UserControls," and "Widgets" are related to the view. Each of these directories likely contains visual components, dialogs, settings, and widgets used to represent the user interface.

- **ViewModel**:

    - HomeViewModel.cs: Specifically, this file appears to be the ViewModel associated with the main page of the application. It contains logic related to the presentation of the user interface and events associated with the loading of the main page.

    - MainPageViewModel.cs: This file seems to be another ViewModel associated with the main page. It includes logic for tab management, keyboard shortcuts, and other interactions related to the main page.

    - SettingsViewModel.cs: This file appears to be the ViewModel associated with the application's settings. It is marked as obsolete, indicating that it should not be used for storing settings, as settings have been moved to IUserSettingsService.

The files "HomeViewModel.cs," "MainPageViewModel.cs," and "SettingsViewModel.cs" represent the View Models in the MVVM pattern.

Exemplifying with code using the file HomeViewModel.cs:

```cs
using Files.App.ViewModels.Widgets;
using Microsoft.UI.Xaml;
using System.Windows.Input;

namespace Files.App.ViewModels
{
	public class HomeViewModel : ObservableObject, IDisposable
	{
		private readonly WidgetsListControlViewModel widgetsViewModel;

		private IShellPage associatedInstance;

		public event EventHandler<RoutedEventArgs> YourHomeLoadedInvoked;

		public ICommand YourHomeLoadedCommand { get; private set; }

		public HomeViewModel(WidgetsListControlViewModel widgetsViewModel, IShellPage associatedInstance)
		{
			this.widgetsViewModel = widgetsViewModel;
			this.associatedInstance = associatedInstance;

			// Create commands
			YourHomeLoadedCommand = new RelayCommand<RoutedEventArgs>(YourHomeLoaded);
		}

		public void ChangeAppInstance(IShellPage associatedInstance)
		{
			this.associatedInstance = associatedInstance;
		}

		private void YourHomeLoaded(RoutedEventArgs e)
		{
			YourHomeLoadedInvoked?.Invoke(this, e);
		}

		public void Dispose()
		{
			widgetsViewModel?.Dispose();
		}
	}
}
```
The `HomeViewModel` is focused on managing the loading of the main page and provides flexibility when changing the associated instance. It handles events related to the loading of the main page of the application. The ViewModel contains fields for a widget list ViewModel (`widgetsViewModel`) and an associated instance (`associatedInstance`). The constructor initializes these fields and creates a command (`YourHomeLoadedCommand`) that executes the `YourHomeLoaded` method when the "home" is loaded, following MVVM pattern conventions.

Regarding the view, an example can be found in the **"LayoutModes" folder, in the file BaseLayoutViewModel.cs**. This file contains logic directly related to user interaction and data manipulation in the user interface.


```cs
// Copyright (c) 2023 Files Community
// Licensed under the MIT License. See the LICENSE.

using Microsoft.UI.Xaml;
using Microsoft.UI.Xaml.Input;
using System.IO;
using System.Windows.Input;
using Windows.ApplicationModel.DataTransfer;
using Windows.ApplicationModel.DataTransfer.DragDrop;
using Windows.Storage;
using Windows.System;

namespace Files.App.ViewModels.LayoutModes
{
	/// <summary>
	/// Represents ViewModel for <see cref="BaseLayout"/>.
	/// </summary>
	public class BaseLayoutViewModel : IDisposable
	{
		private readonly IShellPage _associatedInstance;

		private readonly ItemManipulationModel _itemManipulationModel;

		public ICommand CreateNewFileCommand { get; private set; }

		public ICommand ItemPointerPressedCommand { get; private set; }

		public ICommand PointerWheelChangedCommand { get; private set; }

		public ICommand DragOverCommand { get; private set; }

		public ICommand DropCommand { get; private set; }

		public BaseLayoutViewModel(IShellPage associatedInstance, ItemManipulationModel itemManipulationModel)
		{
			_associatedInstance = associatedInstance;
			_itemManipulationModel = itemManipulationModel;

			CreateNewFileCommand = new RelayCommand<ShellNewEntry>(CreateNewFile);
			ItemPointerPressedCommand = new AsyncRelayCommand<PointerRoutedEventArgs>(ItemPointerPressedAsync);
			PointerWheelChangedCommand = new RelayCommand<PointerRoutedEventArgs>(PointerWheelChanged);
			DragOverCommand = new AsyncRelayCommand<DragEventArgs>(DragOverAsync);
			DropCommand = new AsyncRelayCommand<DragEventArgs>(DropAsync);
		}

		private void CreateNewFile(ShellNewEntry f)
		{
			UIFilesystemHelpers.CreateFileFromDialogResultTypeAsync(AddItemDialogItemType.File, f, _associatedInstance);
		}

		private async Task ItemPointerPressedAsync(PointerRoutedEventArgs e)
		{
			// If a folder item was clicked, disable middle mouse click to scroll to cancel the mouse scrolling state and re-enable it
			if (e.GetCurrentPoint(null).Properties.IsMiddleButtonPressed &&
				e.OriginalSource is FrameworkElement { DataContext: ListedItem Item } &&
				Item.PrimaryItemAttribute == StorageItemTypes.Folder)
			{
				_associatedInstance.SlimContentPage.IsMiddleClickToScrollEnabled = false;
				_associatedInstance.SlimContentPage.IsMiddleClickToScrollEnabled = true;

				if (Item.IsShortcut)
					await NavigationHelpers.OpenPathInNewTab(((e.OriginalSource as FrameworkElement)?.DataContext as ShortcutItem)?.TargetPath ?? Item.ItemPath);
				else
					await NavigationHelpers.OpenPathInNewTab(Item.ItemPath);
			}
		}

		private void PointerWheelChanged(PointerRoutedEventArgs e)
		{
			if (e.KeyModifiers is VirtualKeyModifiers.Control &&
				_associatedInstance.IsCurrentInstance)
			{
				int delta = e.GetCurrentPoint(null).Properties.MouseWheelDelta;

				// Mouse wheel down
				if (delta < 0)
					_associatedInstance.InstanceViewModel.FolderSettings.GridViewSize -= Constants.Browser.GridViewBrowser.GridViewIncrement;
				// Mouse wheel up
				else if (delta > 0)
					_associatedInstance.InstanceViewModel.FolderSettings.GridViewSize += Constants.Browser.GridViewBrowser.GridViewIncrement;

				e.Handled = true;
			}
		}

		public async Task DragOverAsync(DragEventArgs e)
		{
			var deferral = e.GetDeferral();

			if (_associatedInstance.InstanceViewModel.IsPageTypeSearchResults)
			{
				e.AcceptedOperation = DataPackageOperation.None;
				deferral.Complete();

				return;
			}

			_itemManipulationModel.ClearSelection();

			if (FilesystemHelpers.HasDraggedStorageItems(e.DataView))
			{
				e.Handled = true;

				var draggedItems = await FilesystemHelpers.GetDraggedStorageItems(e.DataView);

				var pwd = _associatedInstance.FilesystemViewModel.WorkingDirectory.TrimPath();
				var folderName = Path.IsPathRooted(pwd) && Path.GetPathRoot(pwd) == pwd ? Path.GetPathRoot(pwd) : Path.GetFileName(pwd);

				// more funtions ......
				//               ......
	}           //               ......
}
```

`BaseLayoutViewModel` is responsible for handling commands, events, and specific UI logic associated with the base layout of the application. For instance, it manages the creation of new files, handles pointer events, deals with scroll wheel actions, and facilitates drag-and-drop operations.

Among its functions, the `CreateNewFileCommand` is responsible for initiating the creation of new files, the `ItemPointerPressedCommand` handles pointer clicks by temporarily disabling central click scrolling and opening folders in new tabs when necessary. The `PointerWheelChangedCommand` adjusts the grid view size based on mouse wheel and control key actions, and so on.

In summary, `BaseLayoutViewModel` controls the specific user interface interaction logic for operations related to the base layout of the application.

--- 

For the proper functioning, different classes are in the folder as well as part of the model representing the data and logic of the application.

To access the documentation and source code for both **MVVM** and the **Files** repository, follow these links:



Link to the **ViewMoled** folder: [ViewMoled folder link](https://github.com/files-community/Files/tree/main/src/Files.App/ViewModels)

Link to the **Files** repository: [Files repository link](https://github.com/files-community/Files/tree/main)






