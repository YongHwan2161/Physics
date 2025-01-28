# Update Zoom
- mouse wheel을 움직이면 zoomLevel과 panX, Y를 조정하는 함수이다. 
```c
void UpdateZoom(int delta) {
    float oldZoom = zoomLevel;
    if (delta > 0) {
        zoomLevel = min(zoomLevel * ZOOM_FACTOR, MAX_ZOOM);
    } else {
        zoomLevel = max(zoomLevel / ZOOM_FACTOR, MIN_ZOOM);
    }
  
    // Adjust pan to keep zoom centered on mouse position
    panX = panX * (zoomLevel / oldZoom);
    panY = panY * (zoomLevel / oldZoom);
}
```