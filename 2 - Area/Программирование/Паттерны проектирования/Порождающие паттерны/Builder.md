# Builder

Builder - паттерн, который помогает вынести часть логики построения объекта в рамки класса `Builder` У нас есть некий класс `Builder`, который позволяет собрать объект. При этом мы можем построение этого сложно объекта завернуть в наборы простых методов. Важный аспект этого паттерна, это то, что он должен быть _chainable_, то есть мы можем друг за другом вызывать эти методы. Чтобы сделать это, в конце каждого метода, метод должен возвращать этот класс обратно.

> [!note] Обратите внимание
> В конце каждого цепочки должен быть метод `build`, который сделает "сборку" этого экземпляра

## Пример

У нас есть сервис, которые генерирует изображение по заданному формату.

```ts
enum ImageFormat {
	Png = 'png',
	Jpeg = 'jpeg'
}

interface IResolution {
	width: number;
	height: number;
}

interface IImageConversion extends IResolution {
	format: ImageFormat;
}

class ImageBuilder {
	private formats: ImageFormat[] = [];
	private resolutions: IResolution[] = [];
	
	addPng() {
		if (this.formats.includes(ImageFormat.Png)) {
			return this;
		}
		this.formats.push(ImageFormat.Png)
		return this;
	}
	
	addJpeg() {
		if (this.formats.includes(ImageFormat.Jpeg)) {
			return this;
		}
		this.formats.push(ImageFormat.Jpeg);
		return this;
	}

	addResolution(width: number; height: number) {
		this.resolution.push({ width, height })
		return this;
	}

	build(): IImageConversion[] {
		const res: IImageConversion[] = [];
		for (const r of this.resolutions) {
			for (const f of this.formats) {
				res.push({
					format: f,
					width: r.width,
					height: r.height,
				})
			}
		}
		return res;
	}
}
```