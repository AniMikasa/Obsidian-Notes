## 和差化积与积化和差公式

**记忆口诀**：  
正弦加正弦，正弦在前；正弦减正弦，余弦在前；  
余弦加余弦，余弦并肩；余弦减余弦，负号正弦。
正弦余弦，正弦和；余弦正弦，正弦差；  
正弦正弦，负余弦差；余弦余弦，余弦和。

**公式：**
$$
\sin A + \sin B = 2\sin\left(\frac{A+B}{2}\right)\cos\left(\frac{A-B}{2}\right)
$$
$$
\sin A - \sin B = 2\cos\left(\frac{A+B}{2}\right)\sin\left(\frac{A-B}{2}\right)
$$
$$
\cos A + \cos B = 2\cos\left(\frac{A+B}{2}\right)\cos\left(\frac{A-B}{2}\right)
$$
$$
\cos A - \cos B = -2\sin\left(\frac{A+B}{2}\right)\sin\left(\frac{A-B}{2}\right)
$$
$$
\sin A \cos B = \frac{1}{2}[\sin(A+B) + \sin(A-B)]
$$

$$
\cos A \sin B = \frac{1}{2}[\sin(A+B) - \sin(A-B)]
$$

$$
\sin A \sin B = -\frac{1}{2}[\cos(A+B) - \cos(A-B)]
$$
$$
\cos A \cos B = \frac{1}{2}[\cos(A+B) + \cos(A-B)]
$$

$$
\sin^2 A - \sin^2 B = \sin(A+B)\sin(A-B)
$$

$$
\cos^2 A - \cos^2 B = -\sin(A+B)\sin(A-B)
$$

 积分中的应用：
$$
\int \sin mx \cos nx \, dx = \frac{1}{2}\int [\sin(m+n)x + \sin(m-n)x] \, dx
$$
$$
\int \cos mx \cos nx \, dx = \frac{1}{2}\int [\cos(m+n)x + \cos(m-n)x] \, dx
$$
$$
\int \sin mx \sin nx \, dx = -\frac{1}{2}\int [\cos(m+n)x - \cos(m-n)x] \, dx
$$

和差化积常用于 $\sin\alpha - \sin\beta$ 型极限：
$$
\lim_{x \to a} \frac{\sin f(x) - \sin g(x)}{x-a} = \lim_{x \to a} \frac{2\cos\frac{f(x)+g(x)}{2}\sin\frac{f(x)-g(x)}{2}}{x-a}
$$


## 泰勒公式
$$
e^x = 1 + x + \frac{x^2}{2!} + \frac{x^3}{3!} + \cdots + \frac{x^n}{n!} + o(x^n)
$$

$$
\ln(1+x) = x - \frac{x^2}{2} + \frac{x^3}{3} - \frac{x^4}{4} + \cdots + (-1)^{n-1}\frac{x^n}{n} + o(x^n)
$$

$$
a^x = 1 + x\ln a + \frac{(x\ln a)^2}{2!} + \cdots + \frac{(x\ln a)^n}{n!} + o(x^n)
$$

$$
\sin x = x - \frac{x^3}{3!} + \frac{x^5}{5!} - \frac{x^7}{7!} + \cdots + (-1)^n\frac{x^{2n+1}}{(2n+1)!} + o(x^{2n+2})
$$

$$
\cos x = 1 - \frac{x^2}{2!} + \frac{x^4}{4!} - \frac{x^6}{6!} + \cdots + (-1)^n\frac{x^{2n}}{(2n)!} + o(x^{2n+1})
$$

$$
\tan x = x + \frac{x^3}{3} + \frac{2x^5}{15} + \frac{17x^7}{315} + o(x^8)
$$

$$
\arcsin x = x + \frac{x^3}{6} + \frac{3x^5}{40} + \frac{5x^7}{112} + o(x^8)
$$

$$
\arctan x = x - \frac{x^3}{3} + \frac{x^5}{5} - \frac{x^7}{7} + \cdots + (-1)^n\frac{x^{2n+1}}{2n+1} + o(x^{2n+2})
$$


$$
(1+x)^\alpha = 1 + \alpha x + \frac{\alpha(\alpha-1)}{2!}x^2 + \frac{\alpha(\alpha-1)(\alpha-2)}{3!}x^3 + \cdots + \frac{\alpha(\alpha-1)\cdots(\alpha-n+1)}{n!}x^n + o(x^n)
$$

$$
\frac{1}{1-x} = 1 + x + x^2 + x^3 + \cdots + x^n + o(x^n)
$$

$$
\frac{1}{1+x} = 1 - x + x^2 - x^3 + \cdots + (-1)^n x^n + o(x^n)
$$

$$
\sqrt{1+x} = 1 + \frac{1}{2}x - \frac{1}{8}x^2 + \frac{1}{16}x^3 - \frac{5}{128}x^4 + o(x^4)
$$

$$
\frac{1}{\sqrt{1+x}} = 1 - \frac{1}{2}x + \frac{3}{8}x^2 - \frac{5}{16}x^3 + \frac{35}{128}x^4 + o(x^4)
$$


## 基本积分表

$$
\int x^n \, dx = \frac{x^{n+1}}{n+1} + C \quad (n \neq -1)
$$
$$
\int \frac{1}{x} \, dx = \ln|x| + C
$$
$$
\int e^x \, dx = e^x + C
$$
$$
\int a^x \, dx = \frac{a^x}{\ln a} + C \quad (a > 0, a \neq 1)
$$
$$
\int \sin x \, dx = -\cos x + C
$$
$$
\int \cos x \, dx = \sin x + C
$$
$$
\int \tan x \, dx = -\ln|\cos x| + C = \ln|\sec x| + C
$$
$$
\int \cot x \, dx = \ln|\sin x| + C
$$
$$
\int \sec^2 x \, dx = \tan x + C
$$
$$
\int \csc^2 x \, dx = -\cot x + C
$$
$$
\int \sec x \tan x \, dx = \sec x + C
$$
$$
\int \csc x \cot x \, dx = -\csc x + C
$$
$$
\int \sec x \, dx = \ln|\sec x + \tan x| + C
$$
$$
\int \csc x \, dx = -\ln|\csc x + \cot x| + C
$$
$$
\int \frac{1}{x\sqrt{x^2-1}} \, dx = \operatorname{arcsec}|x| + C
$$
$$
\int \frac{1}{\sqrt{a^2 - x^2}} \, dx = \arcsin\frac{x}{a} + C
$$
$$
\int \frac{1}{x^2 - a^2} \, dx = \frac{1}{2a} \ln\left|\frac{x-a}{x+a}\right| + C
$$
$$
\int \frac{1}{\sqrt{x^2 \pm a^2}} \, dx = \ln\left|x + \sqrt{x^2 \pm a^2}\right| + C
$$
$$
\int \frac{1}{x^2 + a^2} \, dx = \frac{1}{a} \arctan\frac{x}{a} + C
$$
$$
\int \frac{1}{\sqrt{a^2 - x^2}} \, dx = \arcsin\frac{x}{a} + C \quad (|x| < a)
$$
$$
\int \sqrt{a^2 - x^2} \, dx = \frac{x}{2}\sqrt{a^2-x^2} + \frac{a^2}{2}\arcsin\frac{x}{a} + C
$$
$$
\int \sqrt{x^2 \pm a^2} \, dx = \frac{x}{2}\sqrt{x^2 \pm a^2} \pm \frac{a^2}{2}\ln\left|x + \sqrt{x^2 \pm a^2}\right| + C
$$
