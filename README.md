# edusrurl
全国edu将近50万个域名
觉得不错的老铁给个双击评论666，老铁们奥力给

提取主域名脚本如下（url.txt命名为domains.txt）：
# 导入专业域名解析库
import tldextract

def extract_target_domains(input_file, output_file="result_domains.txt"):
    """
    从输入文件中提取目标域名（二级域名+顶级域名）并去重输出
    
    参数：
        input_file: 包含原始域名的文本文件路径（每行一个域名）
        output_file: 结果保存的文件路径（默认：result_domains.txt）
    """
    # 用集合存储结果（自动去重）
    target_domains = set()
    
    try:
        # 1. 读取原始域名文件
        with open(input_file, 'r', encoding='utf-8') as f:
            # 遍历每一行域名（去除换行符和空格）
            for line in f:
                domain = line.strip()
                # 跳过空行
                if not domain:
                    continue
                
                # 2. 解析域名（拆分：子域名、二级域名、顶级域名）
                ext_result = tldextract.extract(domain)
                # ext_result 结构说明：
                # - subdomain: 子域名（如 www、xsc）
                # - domain: 二级域名（如 abc、ahau）
                # - suffix: 顶级域名（如 edu.cn、com）
                
                # 3. 重组目标域名（二级域名 + 顶级域名）
                target_domain = f"{ext_result.domain}.{ext_result.suffix}"
                # 添加到集合（自动去重）
                target_domains.add(target_domain)
        
        # 4. 排序并输出结果（按字母顺序排列，更易读）
        sorted_domains = sorted(target_domains)
        
        # 打印到控制台
        print("提取到的目标域名（去重后）：")
        for idx, dom in enumerate(sorted_domains, 1):
            print(f"{idx}. {dom}")
        
        # 保存到文件
        with open(output_file, 'w', encoding='utf-8') as f:
            for dom in sorted_domains:
                f.write(dom + '\n')
        
        print(f"\n结果已保存到：{output_file}")
    
    except FileNotFoundError:
        print(f"错误：输入文件 '{input_file}' 未找到，请检查文件路径！")
    except Exception as e:
        print(f"处理过程中出现错误：{str(e)}")

# ------------------- 脚本使用入口 -------------------
if __name__ == "__main__":
    # 请替换为你的原始域名文件路径（如 "domains.txt"）
    INPUT_FILE_PATH = "domains.txt"
    # 调用函数执行提取
    extract_target_domains(INPUT_FILE_PATH)
