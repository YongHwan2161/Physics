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
# Update Pan
- 화면을 상하좌우로 확장하여 이동시키기 위해 필요함. zoom level에 따라서 움직이는 양이 변화함.
```c
void UpdatePan(int dx, int dy) {
    // Scale the pan movement by zoom level for consistent feel
    float zoom = GetZoomLevel();
    panX += dx / zoom;
    panY += dy / zoom;
}
```