# OpenGL
- GUI는 OpenGL로 개발한다. 

- 각각의 vertex는 화면에서 하나의 particle로 rendering된다. 그리고 vertex간의 관계는 particle들을 연결하는 직선 또는 곡선의 화살표로 나타낸다. 
# Mouse
## mouse whelel
- mouse wheel을 이용해서 zoom in, zoom out이 가능해야 한다. 
- mouse wheel을 움직일 경우, ScreenToClient 함수를 호출하여 먼저 zoom center를 맞춘다. 
- 그리고 [[Zoom#Update Zoom|update zoom]] 함수를 호출하여 zoom level을 조절한다. 
- 
```c
        case WM_MOUSEWHEEL: {
            // Get mouse position for zoom centering
            POINT pt;
            pt.x = GET_X_LPARAM(lParam);
            pt.y = GET_Y_LPARAM(lParam);
            ScreenToClient(hwnd, &pt);
            
            UpdateZoom(GET_WHEEL_DELTA_WPARAM(wParam));
            InvalidateRect(hwnd, NULL, TRUE);
            return 0;
        }
```
## mouse R click
- mouse의 right button을 click하고 drag하면 화면을 이동시킬 수 있다. 
- mouse movement가 감지될 때 right button이 click되었는지를 검사하여 click된 경우에는 [[Zoom#Update Pan|update pan]] 함수를 호출하여 pan을 조정한다. 
```c
        case WM_MOUSEMOVE: {
            int mouseX = LOWORD(lParam);
            int mouseY = HIWORD(lParam);
            static int lastMouseX = 0;
            static int lastMouseY = 0;
            if (wParam & MK_RBUTTON) {  // Right button is down
                int dx = mouseX - lastMouseX;
                int dy = mouseY - lastMouseY;
                UpdatePan(dx, dy);
                InvalidateRect(hwnd, NULL, TRUE);
                SetCursor(LoadCursor(NULL, IDC_SIZEALL));
            }
```