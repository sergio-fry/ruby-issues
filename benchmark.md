# Benchmark

Issues with ruby stdlib [Benchmark](https://docs.ruby-lang.org/en/master/Benchmark.html)

## Basic

```ruby
# Bad
puts Benchmark.measure { "a"*1_000_000_000 }


# Good
puts Benchmark::Measurement.new { "a"*1_000_000_000 }.to_s
puts Benchmark::Measurement.new { "a"*1_000_000_000 }.report
```


## Comparsion

```ruby
# Bad
n = 5000000
Benchmark.bm do |x|
  x.report { for i in 1..n; a = "1"; end }
  x.report { n.times do   ; a = "1"; end }
  x.report { 1.upto(n) do ; a = "1"; end }
end


# Good
n = 5000000
Benchmark::Comparsion.new do |report|
  report.measure { for i in 1..n; a = "1"; end }
  report.measure { n.times do   ; a = "1"; end }
  report.measure { 1.upto(n) do ; a = "1"; end }
end.report.print_to_stdout
```



## Compartion Multirun
```ruby
# Bad
array = (1..1000000).map { rand }
Benchmark.bmbm do |x|
  x.report("sort!") { array.dup.sort! }
  x.report("sort")  { array.dup.sort  }
end


# Good
Benchmark::MultirunComparsion.new do |report|
  report.measure { for i in 1..n; a = "1"; end }
  report.measure { n.times do   ; a = "1"; end }
  report.measure { 1.upto(n) do ; a = "1"; end }
end

# Good
Benchmark::WithColdRun.new(
  Benchmark::Comparsion.new do |report|
    report.measure { for i in 1..n; a = "1"; end }
    report.measure { n.times do   ; a = "1"; end }
    report.measure { 1.upto(n) do ; a = "1"; end }
  end
).report
```


