# 这是一个Python代码，可以将一个大图片批量剪切为两个小图片，你需要安装Python和相应的IDM，我正在使用Charm，您需要在Python中安装Pillow和So组件

from PIL import Image
import os
def split_image(image_path, output_folder):
    # 打开图片
    img = Image.open(image_path)
    img_width, img_height = img.size

    # 计算新图片的宽度（原始图片的宽度除以2）
    new_width = img_width // 2

    # 获取不带扩展名的原始文件名
    original_filename = os.path.splitext(os.path.basename(image_path))[0]

    # 第一部分图片的坐标（左半部分）
    img1_box = (0, 0, new_width, img_height)
    img1 = img.crop(img1_box)
    img1_path = os.path.join(output_folder, f"{original_filename}_01.jpg")
    img1.save(img1_path)
    print(f"Saved {img1_path}")

    # 第二部分图片的坐标（右半部分）
    img2_box = (new_width, 0, img_width, img_height)
    img2 = img.crop(img2_box)
    img2_path = os.path.join(output_folder, f"{original_filename}_02.jpg")
    img2.save(img2_path)
    print(f"Saved {img2_path}")

def batch_split_images(folder_path, output_folder):
    # 遍历目录下的所有文件
    for filename in os.listdir(folder_path):
        # 构建完整的文件路径
        file_path = os.path.join(folder_path, filename)
        # 检查是否为文件
        if os.path.isfile(file_path):
            # 对文件进行切割处理
            try:
                split_image(file_path, output_folder)
                print(f"Successfully processed {filename}")
            except Exception as e:
                print(f"Failed to process {filename}. Error: {e}")
# 使用示例
source_folder = "D:\\big"
output_folder = "D:\\small"
# 确保输出文件夹存在
os.makedirs(output_folder, exist_ok=True)
batch_split_images(source_folder, output_folder)

