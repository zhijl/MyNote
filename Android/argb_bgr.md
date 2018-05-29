# Bitmap 中 ARGB_8888 转为 BGR_888

``` java
public byte[] getImagePixels(Bitmap image) {
    // calculate how many bytes our image consists of
    int bytes = image.getByteCount();

    ByteBuffer buffer = ByteBuffer.allocate(bytes); // Create a new buffer
    image.copyPixelsToBuffer(buffer); // Move the byte data to the buffer

    byte[] temp = buffer.array(); // Get the underlying array containing the data.

    byte[] pixels = new byte[(temp.length / 4) * 3]; // Allocate for 3 byte BGR

    // Copy pixels into place
    for (int i = 0; i < (temp.length / 4); i++) {
       pixels[i * 3] = temp[i * 4 + 3];     // B
       pixels[i * 3 + 1] = temp[i * 4 + 2]; // G
       pixels[i * 3 + 2] = temp[i * 4 + 1]; // R

       // Alpha is discarded
    }

    return pixels;
}
```