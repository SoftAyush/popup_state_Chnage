# popup_state_Chnage
Future<void> _showImageConfirmationDialog(File imageFile) async {
  double _currentScale = 1.0; // Initial zoom level

  bool? confirmSave = await showDialog<bool>(
    context: context,
    builder: (BuildContext context) {
      return StatefulBuilder( // To rebuild dialog on slider change
        builder: (BuildContext context, StateSetter setState) {
          return AlertDialog(
            title: Text(
              "Confirm",
              textAlign: TextAlign.center,
            ),
            content: Column(
              mainAxisSize: MainAxisSize.min,
              children: [
                ClipOval(
                  child: InteractiveViewer(
                    boundaryMargin: EdgeInsets.all(20),
                    minScale: 1.0,
                    maxScale: 3.0,
                    scaleEnabled: false, // Disable pinch to zoom
                    child: Transform.scale( // Apply zoom scale from slider
                      scale: _currentScale,
                      child: Image.file(
                        imageFile,
                        height: 140,
                        width: 140,
                        fit: BoxFit.cover,
                      ),
                    ),
                  ),
                ),
                SizedBox(height: 10),
                Text("Do you want to change profile picture?"),
                SizedBox(height: 10),
                // Slider to control zoom level
                Row(
                  children: [
                    Text("Zoom:"),
                    Expanded(
                      child: Slider(
                        value: _currentScale,
                        min: 1.0,
                        max: 3.0,
                        divisions: 20, // Optional: step increments
                        label: "${_currentScale.toStringAsFixed(1)}x",
                        onChanged: (value) {
                          setState(() {
                            _currentScale = value;
                          });
                        },
                      ),
                    ),
                  ],
                ),
              ],
            ),
            actions: [
              TextButton(
                onPressed: () => Navigator.of(context).pop(false),
                child: Text("Cancel"),
              ),
              TextButton(
                onPressed: () => Navigator.of(context).pop(true),
                child: Text("Confirm"),
              ),
            ],
          );
        },
      );
    },
  );

  if (confirmSave == true) {
    // Proceed with saving the image if confirmed
    // Your save function here
  }
}
