---
title: Android 筆記 - MVVM 中的 RecyclerView 點擊事件
date: 2022-12-11 10:00:00
tags:
categories:
- Android 筆記
---

自從進系統廠就很久沒碰到正常 Android 的世界了，這次難得有專案可以重回正常的世界。

本次使用 MVVM 架構，RecyclerView 的 Adapter 使用 [ListAdapter](https://developer.android.com/reference/androidx/recyclerview/widget/ListAdapter) 而非 RecyclerView.Adapter，UI 綁定使用 [ViewBinding](https://developer.android.com/topic/libraries/view-binding)

<!--more-->

首先大約描述一下流程：

1. 在 ViewHolder 中點擊後，會取得目前 item 在 Adapter 中的位置 (這裡使用  `absoluteAdapterPosition`)，並回傳至 Adapter
2. Adapter 收到 ViewHolder 的資訊後，會繼續將資訊往上傳送，也就是 Fragment 或 Activity
3. Fragment 或 Activity 收到後，再呼叫 ViewModel 做事

然後有一個 data class 大約長這樣

```kotlin
data class Ticket(
	val ticketCode: Int = 0
)
```

# 設定 ViewHolder

`onClickListener` 需要放在 `init` 中，放在 bind() 會直接不理你。

點擊時呼叫我們設定的 onItemClicked，並帶入 `absoluteAdapterPosition` 把資料用 Callback 傳到 Adapter。

```kotlin
class ViewHolder(private val binding: ItemTicketBinding, onItemClicked: (Int) -> Unit) :
        RecyclerView.ViewHolder(binding.root) {

        init {
            binding.buttonTicketItemSelect.setOnClickListener {
                onItemClicked(absoluteAdapterPosition)
            }
        }

        fun bind(ticket: Ticket) {
            binding.textViewTicketItemName.text = ticket.tickeName
        }
    }
```




# 設定 Adapter

既然 ViewHolder 要傳資料過來，Adapter 在 onCreateViewHolder 時也需要設定可以接受資料的地方。

接收資料後，再次用 Callback 將資料傳到 Fragment 或 Activity

```kotlin
class TicketListAdapter(private val onItemClicked: (String) -> Unit) :
    ListAdapter<Ticket, TicketListAdapter.ViewHolder>(TicketListDiffCallback()) {

    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): ViewHolder {
        val binding = ItemTicketBinding.inflate(LayoutInflater.from(parent.context), parent, false)
        return ViewHolder(binding) { absoluteAdapterPosition ->
            onItemClicked(getItem(absoluteAdapterPosition).ticketCode)
        }
    }

    override fun onBindViewHolder(holder: ViewHolder, position: Int) {
        val item = getItem(position)
        holder.bind(item)
    }

    // ViewHolder here
}
```

# 設定 Fragment 中的 Adapter

最後就可以在 Fragment 或 Activity 中收到我們要的資料。

```kotlin
private fun setupRecyclerView() {
    binding.recyclerViewHomeFrag.adapter = TicketAdapter { ticketCode ->
        viewModel.doSomeAction(ticketCode)
    }
}
```



# Reference

[Add a click listener to your Adapter using MVVM in Kotlin (part 2)](https://medium.com/@gunayadem.dev/add-a-click-listener-to-your-adapter-using-mvvm-in-kotlin-part-2-9dce852e96d5)

[RecyclerView Item Click in a Better Way](https://hamurcuabi.medium.com/recyclerview-item-click-in-a-better-way-c69d9c074ddf)