---
ms.openlocfilehash: 7766a59131fffe2b436c15a5ff58e67001be7941
ms.sourcegitcommit: a69d548f90a03e105ee6701236c38390ecd9ccd1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/14/2020
ms.locfileid: "90065093"
---
### <a name="cryptostreamdispose-transforms-final-block-only-when-writing"></a><span data-ttu-id="85005-101">CryptoStream.Dispose 仅在写入时转换最终块</span><span class="sxs-lookup"><span data-stu-id="85005-101">CryptoStream.Dispose transforms final block only when writing</span></span>

<span data-ttu-id="85005-102">用于完成 `CryptoStream` 操作的 <xref:System.Security.Cryptography.CryptoStream.Dispose%2A?displayProperty=nameWithType> 方法不再尝试在读取时转换最终块。</span><span class="sxs-lookup"><span data-stu-id="85005-102">The <xref:System.Security.Cryptography.CryptoStream.Dispose%2A?displayProperty=nameWithType> method, which is used to finish `CryptoStream` operations, no longer attempts to transform the final block when reading.</span></span>

#### <a name="change-description"></a><span data-ttu-id="85005-103">更改说明</span><span class="sxs-lookup"><span data-stu-id="85005-103">Change description</span></span>

<span data-ttu-id="85005-104">在以前的 .NET 版本中，如果用户在 <xref:System.Security.Cryptography.CryptoStreamMode.Read> 模式下使用 <xref:System.Security.Cryptography.CryptoStream> 时执行了不完整的读取操作，<xref:System.Security.Cryptography.CryptoStream.Dispose%2A> 方法可能会引发异常（例如，使用带有填充的 AES 时）。</span><span class="sxs-lookup"><span data-stu-id="85005-104">In previous .NET versions, if a user performed an incomplete read when using <xref:System.Security.Cryptography.CryptoStream> in <xref:System.Security.Cryptography.CryptoStreamMode.Read> mode, the <xref:System.Security.Cryptography.CryptoStream.Dispose%2A> method could throw an exception (for example, when using AES with padding).</span></span> <span data-ttu-id="85005-105">引发异常是因为尝试转换最终块，但数据不完整。</span><span class="sxs-lookup"><span data-stu-id="85005-105">The exception was thrown because the final block was attempted to be transformed but the data was incomplete.</span></span>

<span data-ttu-id="85005-106">在 .NET Core 3.0 及更高版本中，<xref:System.Security.Cryptography.CryptoStream.Dispose%2A> 不再尝试在读取时转换最终块，这会允许执行不完整的读取操作。</span><span class="sxs-lookup"><span data-stu-id="85005-106">In .NET Core 3.0 and later versions, <xref:System.Security.Cryptography.CryptoStream.Dispose%2A> no longer tries to transform the final block when reading, which allows for incomplete reads.</span></span>

#### <a name="reason-for-change"></a><span data-ttu-id="85005-107">更改原因</span><span class="sxs-lookup"><span data-stu-id="85005-107">Reason for change</span></span>

<span data-ttu-id="85005-108">取消网络操作后，此更改将允许从加密流中进行不完整的读取操作，而无需捕获异常。</span><span class="sxs-lookup"><span data-stu-id="85005-108">This change enables incomplete reads from the crypto stream when a network operation is cancelled, without the need to catch an exception.</span></span>

#### <a name="version-introduced"></a><span data-ttu-id="85005-109">引入的版本</span><span class="sxs-lookup"><span data-stu-id="85005-109">Version introduced</span></span>

<span data-ttu-id="85005-110">3.0</span><span class="sxs-lookup"><span data-stu-id="85005-110">3.0</span></span>

#### <a name="recommended-action"></a><span data-ttu-id="85005-111">建议操作</span><span class="sxs-lookup"><span data-stu-id="85005-111">Recommended action</span></span>

<span data-ttu-id="85005-112">大多数应用都不会受到此更改的影响。</span><span class="sxs-lookup"><span data-stu-id="85005-112">Most apps should not be affected by this change.</span></span>

<span data-ttu-id="85005-113">如果应用程序先前在读取不完整的情况下捕获到异常，你可以删除相应 `catch` 块。</span><span class="sxs-lookup"><span data-stu-id="85005-113">If your application previously caught an exception in case of an incomplete read, you can delete that `catch` block.</span></span>
<span data-ttu-id="85005-114">如果应用在哈希方案中使用了最终块的转换，则可能需要确保在释放所有流之前先对其进行读取。</span><span class="sxs-lookup"><span data-stu-id="85005-114">If your app used transforming of the final block in hashing scenarios, you might need to ensure that the entire stream is read before it's disposed.</span></span>

#### <a name="category"></a><span data-ttu-id="85005-115">类别</span><span class="sxs-lookup"><span data-stu-id="85005-115">Category</span></span>

<span data-ttu-id="85005-116">密码</span><span class="sxs-lookup"><span data-stu-id="85005-116">Cryptography</span></span>

#### <a name="affected-apis"></a><span data-ttu-id="85005-117">受影响的 API</span><span class="sxs-lookup"><span data-stu-id="85005-117">Affected APIs</span></span>

- <xref:System.Security.Cryptography.CryptoStream.Dispose%2A?displayProperty=fullName>

<!--

#### Affected APIs

- `M:System.Security.Cryptography.CryptoStream.Dispose`

-->